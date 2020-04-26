## [![XChart](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_64_64.png)](http://knowm.org/open-source/xchart) XChart

XChart is a light weight Java library for plotting data.

## Description

XChart is a light-weight and convenient library for plotting data designed to go from data to chart in the least amount of time possible and to take the guess-work out of customizing the chart style.

## Simplest Example

Create a `XYChart` instance via `QuickChart`, add a series of data to it, and either display it or save it as a bitmap.  

```java

double[] xData = new double[] { 0.0, 1.0, 2.0 };
double[] yData = new double[] { 2.0, 1.0, 0.0 };

// Create Chart
XYChart chart = QuickChart.getChart("Sample Chart", "X", "Y", "y(x)", xData, yData);

// Show it
new SwingWrapper(chart).displayChart();

// Save it
BitmapEncoder.saveBitmap(chart, "./Sample_Chart", BitmapFormat.PNG);

// or save it in high-res
BitmapEncoder.saveBitmapWithDPI(chart, "./Sample_Chart_300_DPI", BitmapFormat.PNG, 300);
```

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Simplest.png)

## Intermediate Example

Create a `XYChart` via a `XYChartBuilder`, style chart, add a series to it, style series, and display chart.

```java

// Create Chart
XYChart chart = new XYChartBuilder().width(600).height(500).title("Gaussian Blobs").xAxisTitle("X").yAxisTitle("Y").build();

// Customize Chart
chart.getStyler().setDefaultSeriesRenderStyle(XYSeriesRenderStyle.Scatter);
chart.getStyler().setChartTitleVisible(false);
chart.getStyler().setLegendPosition(LegendPosition.InsideSW);
chart.getStyler().setMarkerSize(16);

// Series
chart.addSeries("Gaussian Blob 1", getGaussian(1000, 1, 10), getGaussian(1000, 1, 10));
XYSeries series = chart.addSeries("Gaussian Blob 2", getGaussian(1000, 1, 10), getGaussian(1000, 0, 5));
series.setMarker(SeriesMarkers.DIAMOND);

new SwingWrapper(chart).displayChart();
```

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Intermediate.png)

## Advanced Example

Create a `XYChart` via a `XYChartBuilder`, style chart, add a series to it, add chart to `XChartPanel`, embed in Java Swing App, and display GUI.

```java

// Create Chart
final XYChart chart = new XYChartBuilder().width(600).height(400).title("Area Chart").xAxisTitle("X").yAxisTitle("Y").build();

// Customize Chart
chart.getStyler().setLegendPosition(LegendPosition.InsideNE);
chart.getStyler().setDefaultSeriesRenderStyle(XYSeriesRenderStyle.Area);

// Series
chart.addSeries("a", new double[] { 0, 3, 5, 7, 9 }, new double[] { -3, 5, 9, 6, 5 });
chart.addSeries("b", new double[] { 0, 2, 4, 6, 9 }, new double[] { -1, 6, 4, 0, 4 });
chart.addSeries("c", new double[] { 0, 1, 3, 8, 9 }, new double[] { -2, -1, 1, 0, 1 });

// Schedule a job for the event-dispatching thread:
// creating and showing this application's GUI.
javax.swing.SwingUtilities.invokeLater(new Runnable() {

  @Override
  public void run() {

    // Create and set up the window.
    JFrame frame = new JFrame("Advanced Example");
    frame.setLayout(new BorderLayout());
    frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

    // chart
    JPanel chartPanel = new XChartPanel<XYChart>(chart);
    frame.add(chartPanel, BorderLayout.CENTER);

    // label
    JLabel label = new JLabel("Blah blah blah.", SwingConstants.CENTER);
    frame.add(label, BorderLayout.SOUTH);

    // Display the window.
    frame.pack();
    frame.setVisible(true);
  }
});

```

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Advanced.png)

To make it real-time, simply call `updateXYSeries` on the `XYChart` instance to update the series data, followed by `revalidate()` and `repaint()` on the `XChartPanel` instance to repaint.

## Features

* [x] No *required* additional dependencies
* [x] Multiple Y-Axis charts
* [x] Line charts
* [x] Step charts
* [x] Scatter charts
* [x] Area charts
* [x] Step Area charts
* [x] Bar charts
* [x] Histogram charts
* [x] Pie charts
* [x] Donut charts
* [x] Bubble charts
* [x] Stick charts
* [x] Dial charts
* [x] Radar charts
* [x] OHLC charts
* [x] Box charts
* [x] Heat maps
* [x] Error bars
* [x] Logarithmic axes
* [x] Number, Date, Bubble and Category X-Axis
* [x] Multiple series
* [x] Tool tips
* [x] Extensive customization
* [x] Themes - XChart, GGPlot2, Matlab
* [x] Right-click, Save-As...
* [x] User-defined axes range
* [x] Definable legend placement
* [x] CSV import and export
* [x] High resolution chart export
* [x] Export as PNG, JPG, BMP, GIF with custom DPI setting
* [x] Export SVG, EPS using optional `de.erichseifert.vectorgraphics2d` library
* [x] Export PDF using optional `pdfbox-graphics2d` library
* [x] Real-time charts
* [x] Java 8 and up

## Chart Types

Currently, there are 5 major chart types. Each type has its corresponding `ChartBuilder`, `Styler` and `Series`.

| Chart Type | Builder | Styler | Series | Allowed Data Types | Default Series Render Style |
|---|---|---|---|---|---|
| XYChart | XYChartBuilder | XYStyler | XYSeries | Number, Date | Line |
| CategoryChart | CategoryChartBuilder | CategoryStyler | CategorySeries | Number, Date, String | Bar |
| PieChart | PieChartBuilder | PieStyler | PieSeries | String | Pie |
| BubbleChart | BubbleChartBuilder | BubbleStyler | BubbleSeries | Number, Date | Round |
| DialChart | DialChartBuilder | DialStyler | DialSeries | double  | Round |
| RadarChart | RadarChartBuilder | RadarStyler | RadarSeries | double[] | Round |
| OHLCChart | OHLCChartBuilder | OHLCStyler | OHLCSeries | OHLC with Date | Candle |
| BoxChart | BoxChartBuilder | BoxStyler | BoxSeries | Number, Date, String | Box |
| HeatMapChart | HeatMapChartBuilder | HeatMapStyler | HeatMapSeries | Number, Date, String | -- |

The different Stylers contain chart styling methods specific to the corresponding chart type as well as common styling methods common across all chart types.

### XYChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_XYChart.png)

`XYChart` charts take Date or Number data types for the X-Axis and Number data types for the Y-Axis. For both axes, the tick marks are auto generated to span the range and domain of the data in evenly-spaced intervals. 

Series render styles include: `Line`, `Scatter`, `Area`, `Step` and `StepArea`.

### CategoryChart 

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_CategoryChart.png)

`CategoryChart` charts take Date, Number or String data types for the X-Axis and Number data types for the Y-Axis. For the X-Axis, each category is given its own tick mark.  

Series render styles include: `Bar`, `Line`, `Scatter`, `Area` and `Stick`.

### PieChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_PieChart.png)

`PieChart` charts take String data types for the pie slice name and Number data types for the pie slice value.  

Series render styles include: `Pie` and `Donut`.

### BubbleChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Bubble_Chart.png)

`BubbleChart` charts take Date or Number data types for the X-Axis and Number data types for the Y-Axis and bubble sizes. 

Series render styles include: `Round` and in the near future `Square`.

### DialChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Dial_Chart.png)

`DialChart` charts take a `double` to set the position of the dial pointer.

### RadarChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Radar_Chart.png)

`RadarChart` charts take a `double[]` of values between `0` and `1` to set the position of radar node.

Series render styles include: `Polygon` and  `Circle`.

### OHLCChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_OHLC.png)

`OHLCChart` charts take Date data types for the X-Axis and 4 Number data types for the Y-Axis. For both axes, the tick marks are auto generated to span the range and domain of the data in evenly-spaced intervals.

Series render styles include: `Candle`, `HiLo`.

### BoxChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_BoxChart.png)

`BoxChart` charts take String data (seriesNames) types for the X-Axis and Number data types for the Y-Axis. Each box chart is calculated from the corresponding series yData.
Create a BoxChart via a BoxChartBuilder, style chart, add a series to it.
```java
// Create Chart
BoxChart chart =
	new BoxChartBuilder().title("box plot demo").build();

// Choose a calculation method
chart.getStyler().setBoxplotCalCulationMethod(BoxplotCalCulationMethod.N_LESS_1_PLUS_1);
chart.getStyler().setToolTipsEnabled(true);

// Series
chart.addSeries("boxOne", Arrays.asList(1,2,3,4));
new SwingWrapper<BoxChart>(chart).displayChart();
```
Four calculation methods for boxplots:  
- "N_PLUS_1": determine the position of the quartile, where Qi is = i (n + 1) / 4, where i = 1, 2, and 3. n represents the number of items contained in the sequence.
Calculate the corresponding quartile based on location.
- "N_LESS_1": Determine the position of the quartile, where Qi is = i (n-1) / 4, where i = 1, 2, and 3. n represents the number of items contained in the sequence.
Calculate the corresponding quartile based on location.
- "NP": Determine the position of the quartile, where Qi is np = (i * n) / 4, where i = 1, 2, and 3. n represents the number of items contained in the sequence.
If np is not an integer, Qi = X [np + 1];
If np is an integer, Qi = (X [np] + X [np + 1]) / 2.
- "N_LESS_1_PLUS_1": Determine the position of the quartile, where Qi is = i (n-1) / 4 + 1, where i = 1, 2, 3. n represents the number of items contained in the sequence.
Calculate the corresponding quartile based on location.

Interquartile range, IQR = Q3-Q1.

Upper whisker = Q3 + 1.5 * IQR = Q3 + 1.5 * (Q3 - Q1), if Upper whisker is greater than the maximum value of yData, Upper whisker = maximum value of yData.

Lower whisker = Q1 - 1.5 * IQR = Q1 - 1.5 * (Q3 -Q1), if the lower whisker is less than the minimum value of yData, the lower whisker = the minimum value of yData.

E.g:

An example of a set of sequence numbers: 12, 15, 17, 19, 20, 23, 25, 28, 30, 33, 34, 35, 36, 37
- "N_PLUS_1":
Q1's position = (14 + 1) /4=3.75,
Q1 = 0.25 × third term + 0.75 × fourth term = 0.25 × 17 + 0.75 × 19 = 18.5;
- "N_LESS_1":
Q1's location = (14-1) /4=3.25,
Q1 = 0.75 × third term + 0.25 × fourth term = 0.75 × 17 + 0.25 × 19 = 17.5;
- "NP":
Q1's position = 14 * 0.25 = 3.5,
Q1 = 19;
- "N_LESS_1_PLUS_1":
Q1's location = (14-1) / 4 + 1 = 4.25
Q1 = 0.75 × the fourth term + 0.25 × the fifth term = 0.75 × 19 + 0.25 × 20 = 19.25.

### HeatMapChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_HeatMap.png)

`HeatMapChart` take Date, Number or String data types for the X-Axis, Y-Axis. 

## Real-time Java Charts using XChart

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_SimpleRealtime.gif)

Creating real-time charts is as simple as calling `updateXYSeries` for one or more series objects through the `XYChart` instance and triggering a redraw of the `JPanel` containing the chart. This works for all chart types including `XYChart`, `CategoryChart`, `BubbleChart` and `PieChart`, for which example source code can be found [here](https://github.com/knowm/XChart/tree/develop/xchart-demo/src/main/java/org/knowm/xchart/demo/charts/realtime). Examples demonstrate using the `SwingWrapper` with `repaintChart()` method as well as `XChartPanel` with `revalidate()` and `repaint()`. 

The following sample code used to generate the above real-time chart can be found [here](https://github.com/knowm/XChart/blob/develop/xchart-demo/src/main/java/org/knowm/xchart/standalone/readme/SimpleRealTime.java).

```java
public class SimpleRealTime {

  public static void main(String[] args) throws Exception {

    double phase = 0;
    double[][] initdata = getSineData(phase);

    // Create Chart
    final XYChart chart = QuickChart.getChart("Simple XChart Real-time Demo", "Radians", "Sine", "sine", initdata[0], initdata[1]);

    // Show it
    final SwingWrapper<XYChart> sw = new SwingWrapper<XYChart>(chart);
    sw.displayChart();

    while (true) {

      phase += 2 * Math.PI * 2 / 20.0;

      Thread.sleep(100);

      final double[][] data = getSineData(phase);

      javax.swing.SwingUtilities.invokeLater(new Runnable() {

        @Override
        public void run() {

          chart.updateXYSeries("sine", data[0], data[1], null);
          sw.repaintChart();
        }
      });
    }

  }

  private static double[][] getSineData(double phase) {

    double[] xData = new double[100];
    double[] yData = new double[100];
    for (int i = 0; i < xData.length; i++) {
      double radians = phase + (2 * Math.PI / xData.length * i);
      xData[i] = radians;
      yData[i] = Math.sin(radians);
    }
    return new double[][] { xData, yData };
  }
}
```

## Chart Customization

All the styling options can be found in one of two possible places: 1) the Chart's `Styler` or 2) the series' `set` methods. With this chart customization design, all customization options can be quickly "discovered" using an IDE's built in "Content Assist". With centralized styling like this, there is no need to hunt around the entire charting API to find that one customization you're looking for - it's all right in one spot!

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Chart_Customization.png)
 
![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Series_Customization.png)
 

### Customizing Axis Tick Values & Axis Tick Labels

XChart automatically creates axis ticks and axis labels. 

Default axis placement can be altered with `chart.getStyler().setXAxisTickMarkSpacingHint(spacingHint);`. 

Default axis label patterns can be altered with one of:
- `chart.getStyler().setDatePattern(datePattern)` 
- `chart.getStyler().setXAxisDecimalPattern(pattern);` 
- `chart.getStyler().setYAxisDecimalPattern(pattern);`


You can also create custom axis placements and axis labels. Create a map containing x -> label mappings:
- x : value where the tick will be drawn (this value is in xData space, not in pixel space). 
- label: Tick label. If it is `null`, tick will be generated with a `" "` label.

```java
      Map<Object, Object> customXAxisTickLabelsMap = new HashMap<>();
      customXAxisTickLabelsMap.put(0, "zero");
      customXAxisTickLabelsMap.put(3, "3.5");
      customXAxisTickLabelsMap.put(5, " ");
      customXAxisTickLabelsMap.put(9, "nine");
      chart.setXAxisLabelOverrideMap(customXAxisTickLabelsMap);

      Map<Object, Object> customYAxisTickLabelsMap = new HashMap<>();
      customYAxisTickLabelsMap.put(1.0, "max c");
      customYAxisTickLabelsMap.put(6.0, "max b");
      customYAxisTickLabelsMap.put(9.0, "max a");
      chart.setYAxisLabelOverrideMap(customYAxisTickLabelsMap);
```

For category charts another way to create custom axis places is using category names in first series:
```java
     Map<Object, Object> tickLabelOverrideMap = new HashMap<Object, Object>();

      Map<Object, Object> customTickLabelsMap = new HashMap<>();
      customTickLabelsMap.put("A", "-A-");
      customTickLabelsMap.put("D", "+D+");
      chart.setXAxisLabelOverrideMap(customTickLabelsMap);
```

Whenever you use `setXAxisLabelOverrideMap` the auto-generated tick labels will be replaced meaning none of the tick labels will be shown besides the ones provided by you in the override map.

### Multiple Axes

XChart has multiple y axes feature. Y offset is calculated according to the y axis the series configured. Max `y` value in this axis is calculated according to the series on this axis only. 
To set the y group: 

```java
    series.setYAxisGroup(axisGroup);   
```

To manually change max/min of axis group: 

```java
    ((AxesChartStyler) chart.getStyler()).setYAxisMax(axisGroup, 200.0);
```

Axis can be drawn on the left (default) or on the right of the chart: 

```java
    chart.getStyler().setYAxisGroupPosition(axisGroup, Styler.YAxisPosition.Right);
```

To set the Y axes titles:

```java
    chart.setYAxisGroupTitle(0, "A");
    chart.setYAxisGroupTitle(1, "B");
```

## Chart Themes

XChart ships with three different themes: Default `XChart`, `GGPlot2` and `Matlab`. Using a different theme is as simple as setting the Chart's theme with the `theme` method of the `ChartBuilder`.

    XYChart chart = new XYChartBuilder().width(800).height(600).theme(ChartTheme.Matlab).build();

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Themes.png)

## What's Next?

Now go ahead and [study some more examples](http://knowm.org/open-source/xchart/xchart-example-code/), [download the thing](http://knowm.org/open-source/xchart/xchart-change-log) and [provide feedback](https://github.com/knowm/XChart/issues).
 
## Getting Started

### Non-Maven

Download Jar: http://knowm.org/open-source/xchart/xchart-change-log

### Maven

The XChart release artifacts are hosted on Maven Central.

Add the XChart library as a dependency to your pom.xml file:

```xml
    <dependency>
        <groupId>org.knowm.xchart</groupId>
        <artifactId>xchart</artifactId>
        <version>3.6.3</version>
    </dependency>
```

For snapshots, add the following to your pom.xml file:

```xml
    <repository>
      <id>sonatype-oss-snapshot</id>
      <snapshots/>
      <url>https://oss.sonatype.org/content/repositories/snapshots</url>
    </repository>

    <dependency>
      <groupId>org.knowm.xchart</groupId>
      <artifactId>xchart</artifactId>
      <version>3.6.4-SNAPSHOT</version>
    </dependency>
```

Snapshots can be manually downloaded from Sonatype: [https://oss.sonatype.org/content/groups/public/org/knowm/xchart/xchart/](https://oss.sonatype.org/content/groups/public/org/knowm/xchart/xchart/)

### SBT

To use XChart with the Scala Build Tool (SBT) add the following to your build.sbt

```scala
libraryDependencies += "org.knowm.xchart" % "xchart" % "3.6.3" exclude("de.erichseifert.vectorgraphics2d", "VectorGraphics2D") withSources()
```

## Building with Maven

#### general

    mvn clean package  
    mvn javadoc:aggregate  

#### Formatting

    mvn com.coveo:fmt-maven-plugin:format
    
Formats your code using [google-java-format](https://github.com/google/google-java-format) which follows [Google's code styleguide](https://google.github.io/styleguide/javaguide.html).

If you want your IDE to stick to the same format, check out the available configuration plugins:

#### Eclipse

Download [`google-java-format-eclipse-plugin_*.jar`](https://github.com/google/google-java-format/releases) and place in `/Applications/Eclipse Java.app/Contents/Eclipse/dropins`. Restart Eclipse. Select the plugin in `Preferences > Java > Code Style > Formatter > Formatter Implementation`. 

#### IntelliJ

In the plugins section in IntelliJ search for `google-java-format` and install the plugin. Restart IntelliJ.

## Running Demo

- Linux: execute command `java -cp xchart-demo-3.6.3.jar:xchart-3.6.3.jar org.knowm.xchart.demo.XChartDemo`.

- Windows: In the cmd command window, execute the command `java -cp xchart-demo-3.6.3.jar;xchart-3.6.3.jar org.knowm.xchart.demo.XChartDemo`; In the PowerShell command window, execute the command `java -cp "xchart-demo-3.6.3.jar;xchart-3.6.3.jar" org.knowm.xchart.demo.XChartDemo`.

E.g:
```sh
cd /path/to/xchart-demo/jar/
java -cp xchart-demo-3.6.3.jar:xchart-3.6.3.jar org.knowm.xchart.demo.XChartDemo
```

![](https://raw.githubusercontent.com/knowm/XChart/develop/etc/XChart_Demo.png)

## Bugs

Please report any bugs or submit feature requests to [XChart's Github issue tracker](https://github.com/knowm/XChart/issues).  

## Continuous Integration

[![Build Status](https://travis-ci.org/knowm/XChart.png?branch=develop)](https://travis-ci.org/timmolter/XChart.png)  
[Build History](https://travis-ci.org/knowm/XChart/builds)  

