## 绘制

许多插件，比如`buttons`，自己就做了它们所有的绘制工作。比如你仅仅需要告诉它们你想看到的标签、你想它们使用的字体、绘制按钮的轮廓和焦点矩形。有时候，有必要做些自定义的绘制。在这种情况下，一个 `GtkDrawingArea`控件可能是正确的选择，这个控件提供了一个画布，在这个画布上你可以绘制并且将其连接到`”draw“`信号。

控件的内容常常需要被部分或者全部重新绘制。比如，当另一个窗口控件被移动并且露出控件的一部分，或者当包含它的窗口重新调整大小时，也会导致控件的部分或者全部被重新绘制。通过调用 `gtk_widget_queue_draw()`或者它的变体，GTK+提供一个现成的`cairo`给绘制信号从而实现众多细节。

下面的程序将会展示一个绘制信号句柄。这个例子比之前的略微复杂，因为它也通过`button_press`和`motion_notify`句柄显示出输入活动。

![drawingpng](../images/drawing.png)

**Example 3. Drawing in response to input**

新建一个名为 `example-3.c` 的文件，写入如下内容：

```c
#include <gtk/gtk.h>

/* Surface to store current scribbles */
static cairo_surface_t *surface = NULL;

static void clear_surface (void)
{
    cairo_t *cr;

    cr = cairo_create (surface);

    cairo_set_source_rgb (cr, 1, 1, 1);
    cairo_paint (cr);

    cairo_destroy (cr);

}

/* Create a new surface of the appropriate size to store our scribbles */
static gboolean
configure_event_cb (GtkWidget         *widget,
                    GdkEventConfigure *event,
                    gpointer           data)
{
    if (surface)
    cairo_surface_destroy (surface);

    surface = gdk_window_create_similar_surface (gtk_widget_get_window (widget),
                                                 CAIRO_CONTENT_COLOR,
                                                 gtk_widget_get_allocated_width (widget),
                                                 gtk_widget_get_allocated_height (widget));

    /* Initialize the surface to white */
    clear_surface ();

    /* We've handled the configure event, no need for further processing. */
    return TRUE;

}

/* Redraw the screen from the surface. Note that the ::draw
*  * signal receives a ready-to-be-used cairo_t that is already
*   * clipped to only draw the exposed areas of the widget
*    */
static gboolean draw_cb (GtkWidget *widget,
         cairo_t   *cr,
         gpointer   data)
{
    cairo_set_source_surface (cr, surface, 0, 0);
    cairo_paint (cr);

    return FALSE;

}

 /* Draw a rectangle on the surface at the given position */
 static void draw_brush (GtkWidget *widget,
             gdouble    x,
             gdouble    y)
 {
     cairo_t *cr;

     /* Paint to the surface, where we store our state */
     cr = cairo_create (surface);

     cairo_rectangle (cr, x - 3, y - 3, 6, 6);
     cairo_fill (cr);

     cairo_destroy (cr);

     /* Now invalidate the affected region of the drawing area. */
     gtk_widget_queue_draw_area (widget, x - 3, y - 3, 6, 6);

 }

 /* Handle button press events by either drawing a rectangle
 * or clearing the surface, depending on which button was pressed.
 * The ::button-press signal handler receives a GdkEventButton
 * struct which contains this information.
 */
 static gboolean button_press_event_cb (GtkWidget      *widget,
                        GdkEventButton *event,
                        gpointer        data)
 {
     /* paranoia check, in case we haven't gotten a configure event */
     if (surface == NULL)
     return FALSE;

     if (event->button == GDK_BUTTON_PRIMARY)
     {
         draw_brush (widget, event->x, event->y);
     }
     else if (event->button == GDK_BUTTON_SECONDARY)
     {
         clear_surface ();
         gtk_widget_queue_draw (widget);

     }

     /* We've handled the event, stop processing */
     return TRUE;

 }

 /* Handle motion events by continuing to draw if button 1 is
 * still held down. The ::motion-notify signal handler receives
 * a GdkEventMotion struct which contains this information.
 */
 static gboolean
 motion_notify_event_cb (GtkWidget      *widget,
                         GdkEventMotion *event,
                         gpointer        data)
 {
     /* paranoia check, in case we haven't gotten a configure event */
     if (surface == NULL)
     return FALSE;

     if (event->state & GDK_BUTTON1_MASK)
     draw_brush (widget, event->x, event->y);

     /* We've handled it, stop processing */
     return TRUE;

 }

  static void close_window (void)
  {
      if (surface)
      cairo_surface_destroy (surface);

      gtk_main_quit ();

  }

  int main (int   argc, char *argv[])
  {
      GtkWidget *window;
      GtkWidget *frame;
      GtkWidget *da;

      gtk_init (&argc, &argv);

      window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
      gtk_window_set_title (GTK_WINDOW (window), "Drawing Area");

      g_signal_connect (window, "destroy", G_CALLBACK (close_window), NULL);

      gtk_container_set_border_width (GTK_CONTAINER (window), 8);

      frame = gtk_frame_new (NULL);
      gtk_frame_set_shadow_type (GTK_FRAME (frame), GTK_SHADOW_IN);
      gtk_container_add (GTK_CONTAINER (window), frame);

      da = gtk_drawing_area_new ();
      /* set a minimum size */
      gtk_widget_set_size_request (da, 100, 100);

      gtk_container_add (GTK_CONTAINER (frame), da);

      /* Signals used to handle the backing surface */
      g_signal_connect (da, "draw",
                        G_CALLBACK (draw_cb), NULL);
      g_signal_connect (da,"configure-event",
                        G_CALLBACK (configure_event_cb), NULL);

      /* Event signals */
      g_signal_connect (da, "motion-notify-event",
                        G_CALLBACK (motion_notify_event_cb), NULL);
      g_signal_connect (da, "button-press-event",
                        G_CALLBACK (button_press_event_cb), NULL);

      /* Ask to receive events the drawing area doesn't normally
      * subscribe to. In particular, we need to ask for the
      * button press and motion notify events that want to handle.
      */
      gtk_widget_set_events (da, gtk_widget_get_events (da)
                             | GDK_BUTTON_PRESS_MASK
                             | GDK_POINTER_MOTION_MASK);

      gtk_widget_show_all (window);

      gtk_main ();

      return 0;
  }
```

然后在终端输入以下命令用GCC编译程序：

```bash
gcc `pkg-config --cflags gtk+-3.0` -o example-3 example-3.c `pkg-config --libs gtk+-3.0`
```

