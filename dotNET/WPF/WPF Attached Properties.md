### Example

```csharp
public static class AnimationBehavior
{
    public static readonly DependencyProperty IsAnimatingProperty =
        DependencyProperty.RegisterAttached("IsAnimating", typeof(bool), typeof(AnimationBehavior),
            new PropertyMetadata(false, OnIsAnimatingChanged));

    public static bool GetIsAnimating(UIElement element) => (bool)element.GetValue(IsAnimatingProperty);
    public static void SetIsAnimating(UIElement element, bool value) => element.SetValue(IsAnimatingProperty, value);

    private static void OnIsAnimatingChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        if (d is UIElement element)
        {
            if ((bool)e.NewValue)
            {
                var animation = new DoubleAnimation
                {
                    From = 1.0,
                    To = 1.2,
                    Duration = TimeSpan.FromSeconds(0.5),
                    AutoReverse = true,
                    RepeatBehavior = RepeatBehavior.Forever
                };

                var transform = new ScaleTransform(1.0, 1.0);
                element.RenderTransform = transform;
                element.RenderTransformOrigin = new Point(0.5, 0.5);
                transform.BeginAnimation(ScaleTransform.ScaleXProperty, animation);
                transform.BeginAnimation(ScaleTransform.ScaleYProperty, animation);
            }
            else
            {
                var transform = element.RenderTransform as ScaleTransform;
                transform?.BeginAnimation(ScaleTransform.ScaleXProperty, null);
                transform?.BeginAnimation(ScaleTransform.ScaleYProperty, null);
            }
        }
    }
}
```

### Using Attached Properties:

```xml
<Button Content="Animate Me"
        local:AnimationBehavior.IsAnimating="True"
        Width="100" Height="50" />
```
