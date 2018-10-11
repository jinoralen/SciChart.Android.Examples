# Custom text formatting for CategoryDateAxis

In case of CategoryDateAxis you need to customize [LabelProvider](https://www.scichart.com/documentation/android/v2.x/webframe.html#Axis%20Labels%20-%20LabelProvider%20API.html) because default LabelProvider implementation ignores TextFormatting provided by axis and selects formatting from one of predefined values based on zoom depth ( so when you zoom to hours it uses 'HH:mm' formatting, when you zoom out it uses 'dd MMM' etc).

You can do it with code like this:

```java
public class CustomTradeChartAxisLabelProvider extends TradeChartAxisLabelProvider {
       public CustomTradeChartAxisLabelProvider() {
           super(new CustomTradeChartAxisLabelFormatter());
       }

       private static class CustomTradeChartAxisLabelFormatter extends TradeChartAxisLabelFormatter {
           private final SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd", Locale.getDefault());

           @Override
           public CharSequence formatLabel(Comparable dataValue) {
               final Date dateToFormat = ComparableUtil.toDate(dataValue);

               // need to synchronize because SimpleDateFormat isn't thread safe
               synchronized (dateFormat) {
                   return dateFormat.format(dateToFormat);
               }
           }
       }
   }
```
