# How to prevent hiding of Rollover/CursorModifier on touch up

To do this just need to extend Rollover/CursorModifier and override their [onTouchDown()](https://www.scichart.com/documentation/android/v2.x/topic604.html) and [onTouchUp()](https://www.scichart.com/documentation/android/v2.x/SciChart.Charting~com.scichart.charting.modifiers.MasterSlaveTouchModifierBase~onTouchUp.html)methods so when first touch down occurs modifier should show tooltip and after that just update tooltip position without hiding tooltip.

For [RolloverModifier](https://www.scichart.com/documentation/android/v2.x/webframe.html#RolloverModifier.html) code will look like this ( for [CursorModifier](https://www.scichart.com/documentation/android/v2.x/webframe.html#CursorModifier.html) code will be the same, just need to extend other base class ):

```java
class CustomRolloverModifier extends RolloverModifier {
        private boolean isFirstTouch = true;
 
        @Override
        protected boolean onTouchDown(ModifierTouchEventArgs args) {
            // place tooltip on first touch only and ignore other touch down events because tooltip should remain on screen
            if(isFirstTouch) {
                isFirstTouch = false;
                return super.onTouchDown(args);
            } else
                super.onTouchMove(args);
            return true;
 
        }
 
        @Override
        protected boolean onTouchUp(ModifierTouchEventArgs args) {
            // ignore touch up event
            return false;
        }
    }
```














