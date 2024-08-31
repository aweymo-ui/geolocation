---
title: Aerial Timelapse
nav: Aerial Timelapse
gallery: true
---

**Another challenge you may encounter** in geolocation is identifying photographs that contain structures that have since been demolished. Creating a time lapse of historical aerial photography can help you better understand when and where larger features such bridges, buildings and natural areas were still standing. 

{% include feature/nav-menu.html sections="Platform Comparison;Google Earth Engine;Output Comparison" %}

## Platform Comparison

While there have been aerial photography initiatives active for over 100 years and major digital repositories now contain over 900 collections (referred to as ‘datasets’ in the GIS discipline), compositing this data together to form a comprehensive time lapse of a defined area is surprisingly difficult. Here are a few different freely available resources and their different strengths and weaknesses. 


**Beginning with a few bad ones** you don’t want to spend your time on (like I did):

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <th style="border: 1px solid black; padding: 8px; width: 15%;">Platform</th>
    <th style="border: 1px solid black; padding: 8px; width: 28.33%;">Strengths</th>
    <th style="border: 1px solid black; padding: 8px; width: 28.33%;">Weaknesses</th>
    <th style="border: 1px solid black; padding: 8px; width: 28.33%;">Directions</th>
  </tr>
  <tr>
    <td style="border: 1px solid black; padding: 8px;">Google Earth Pro</td>
    <td style="border: 1px solid black; padding: 8px;"></td>
    <td style="border: 1px solid black; padding: 8px;"><li>This desktop version of Google Earth is where some older online references will point you towards for this functionality, but this is an outdated model that Google appears to have abandoned for more contemporary platforms</li> <li>The “Movie Maker” tool that would carry out this action no longer appears to be functional</li></td>
    <td style="border: 1px solid black; padding: 8px;"></td>
  </tr>
  <tr>
    <td style="border: 1px solid black; padding: 8px;">USGS Earth Explorer</td>
    <td style="border: 1px solid black; padding: 8px;">If you want to source aerial photographs and put them into a time lapse manually using a resource like Adobe Premiere or After Effects, this database contains material that predates many of the other resources, sometimes as far back as 1950, whereas the others only reach as far back as 1980.</td>
    <td style="border: 1px solid black; padding: 8px;"><li>As mentioned, only still images can be exported, so the time lapse function and overlaying images need to be done manually</li> <li>Extremely non-intuitive interface.</li></td>
    <td style="border: 1px solid black; padding: 8px;"></td>
  </tr>
  <tr>
    <td style="border: 1px solid black; padding: 8px;"><a href="https://earth.google.com/web/search/Moscow,+ID/@46.73489678,-117.00088295,787.89618213a,6616.90720226d,35y,0h,0t,0r/data=CjciJgokCWEEJ1ALx0dAEZtDN-Eme0ZAGeDG8ZNcE1zAISGjYGkCQ13AOg0IAREAAAAAAADgPyABOgMKATA">Google Earth</a></td>
    <td style="border: 1px solid black; padding: 8px;">
        <ul>
            <li>Very simple to use</li>
            <li>Good date range, reaching all the way back to 1984 in this example</li>
            <li>Landmark names help orient the viewer</li>
        </ul>
    </td>
    <td style="border: 1px solid black; padding: 8px;">
        <ul>
            <li>Very poor fidelity</li>
            <li>No video export functionality</li>
        </ul>
    </td>
    <td style="border: 1px solid black; padding: 8px;">
        <ol>
            <li>Enter your location</li>
            <li>Select the Layers button on the bottom left hand of the screen</li>
            <li>Toggle the time lapse button</li>
            <li>Adjust speed as wanted</li>
            <li>Screen capture to create video</li>
        </ol>
    </td>
  </tr>
  <tr>
    <td style="border: 1px solid black; padding: 8px;"><a href="https://livingatlas.arcgis.com/wayback/#active=39767&mapCenter=-116.99564%2C46.73436%2C15&animationSpeed=1000">ArcGIS World Imagery Wayback</a></td>
    <td style="border: 1px solid black; padding: 8px;">
        <ul>
            <li>Better fidelity</li>
            <li>Even more intuitive controls</li>
        </ul>
    </td>
    <td style="border: 1px solid black; padding: 8px;"><li>Historical reach isn’t very far; only about 10 years depending on the area.</li></td>
    <td style="border: 1px solid black; padding: 8px;">
        <ol>
            <li>Enter location</li>
            <li>Press the play button and adjust speed as desired</li>
            <li>Export video directly or upload to cloud</li>
        </ol>
    </td>
  </tr>
</table>

<div class="symbol-container">
    <p class="symbol">&#10042;</p>
</div>

## Google Earth Engine

While I did find the learning curve on this resource to be steep, its adaptability, variety of datasets and convenient export settings make this a good geolocation resource to end on. Like the USGS Earth Explorer, you will need to sign a waiver that you will be using the resource for non-profit reasons, but after signing you should have near immediate access. Once inside, the interface looks a bit like a text editor if you do any coding. On the left pane you have the scripts that you’ve written along with examples of other scripts you can automatically generate to help you learn the programmatic language as well as the Google Earth Engine Docs to drill down on specific functionalities. The center pane is where you will write and run the code. On the right pane your console acts essentially as a terminal where error messages will feed out and also where data will be printed after running a script and the task pane will have a list of the scripts you have run along with their status.

The intention of the code was to create a time lapse over Moscow, ID that would span the greatest amount of time. 

```
// Define the center of the 83843 area code in Moscow, Idaho
var moscowCenter = ee.Geometry.Point([-117.0028, 46.7326]); // Approximate center of 83843

// Define a square boundary around Moscow, covering an area of interest (e.g., 5 square miles ~ 12.95 square km)
var moscowBoundary = moscowCenter.buffer(1810).bounds(); // 1810 meters radius approximates a 5 square mile area

// Load the ImageCollection and filter by the defined area
var collection = ee.ImageCollection('USDA/NAIP/DOQQ')
  .filterBounds(moscowBoundary)
  .sort('system:time_start');
  ```

  To begin, we are defining our perimeter geographically around the 83843 area code. Next we are going to define the dataset we want to pull from to create this visualization. While Google Earth Engine contains over 900 datasets, for a beginner like me, I was unable to combine datasets to increase the span of time in any given area. I believe this is due to the datasets having slightly different metadata and contrasting color outputs so, for this script, we are only pulling from the National Agriculture Imagery Program dataset, which goes back to about 1980 and in line 13 you can see I am requesting the earliest possible image of this defined area. 

  ```
// Get the earliest image
var earliestImage = collection.first();
var earliestDate;

if (earliestImage) {
  earliestDate = ee.Date(earliestImage.get('system:time_start')).format('YYYY-MM-dd').getInfo();
  print('Earliest Available Date:', earliestDate);

  // Define the updated date range
  var startDate = earliestDate; // Use earliest date found
  var endDate = '2024-06-01'; // End date

  // Filter the ImageCollection with the updated date range
  collection = collection.filterDate(startDate, endDate);

  // Function to get the image with the largest area for each year
  var getLargestImagePerYear = function(year) {
    var yearlyCollection = collection.filter(ee.Filter.calendarRange(year, year, 'year'));
    
    // Compute the intersection of each image with the boundary and calculate area
    var largestImage = yearlyCollection.map(function(image) {
      var intersection = image.geometry().intersection(moscowBoundary, 1); // Added error margin of 1 meter
      return image.set({'intersectionArea': intersection.area()});
    }).sort('intersectionArea', false).first(); // Sort by area and get the largest

    return largestImage;
  };

  // Get the years from the collection
  var years = ee.List(collection.aggregate_array('system:time_start'))
    .map(function(date) {
      return ee.Date(date).get('year');
    }).distinct();

  // Map over years to get the largest image per year
  var largestImagesCollection = ee.ImageCollection(years.map(getLargestImagePerYear))
    .sort('system:time_start'); // Ensure chronological order
```

Moving  to line 28, another complication that I encountered was that not all of the satellite photographs that capture part of the area necessarily cover all of it, so some of the early iterations of this script had little slivers of the terrain that were unhelpful in understanding geographic change over time. To solve this I am asking in code for just the largest image for each year to balance getting the best coverage with the best yield of images. Starting around line 41, we are asking in code to assemble these images chronologically from earliest to most recent and compile them in a video format and export them in red, green and blue color formatting. 

```
 // Function to prepare images for video export and add date metadata
  var prepareForExport = function(image) {
    var date = ee.Date(image.get('system:time_start')).format('YYYY-MM-dd');
    
    // Visualize the image with appropriate bands and scale
    var visualImage = image.visualize({
      bands: ['R', 'G', 'B'],
      min: 0,
      max: 255
    });
    
    // Annotate the image with the date (add metadata)
    return visualImage.set({'date': date});
  };

  // Apply the function to the largest images collection
  var annotatedCollection = largestImagesCollection.map(prepareForExport);

  // Print dates of all images to the console in chronological order
  annotatedCollection.aggregate_array('date').evaluate(function(dates) {
    print('Image Dates:', dates);
  }, function(error) {
    print('Error:', error);
  });
```
**In a perfect world** I was hoping to create a code that would automatically overlay the date information of each photograph over its image, so the viewer would have a better frame of reference, but the best I was able to do was have this date information print when the script is run in the console. This information can then be fairly easily added over the frames using video editing software like Adobe Premiere
```
  // Define video export parameters with increased display time
  var videoParams = {
    region: moscowBoundary,
    dimensions: 900, // Adjust as needed
    crs: 'EPSG:3857',
    framesPerSecond: 0.5, // Increase time on screen to 2 seconds per image
    description: 'Moscow_TimeLapse',
    folder: 'EarthEngine'
  };

  // Export the video
  Export.video.toDrive({
    collection: annotatedCollection,
    description: videoParams.description,
    folder: videoParams.folder,
    dimensions: videoParams.dimensions,
    framesPerSecond: videoParams.framesPerSecond,
    crs: videoParams.crs,
    region: videoParams.region
  });

} else {
  print('No images found within the specified boundary.');
}
```

Finally, one of the convenient things about working with Google Earth Engine is that you can export videos, images and interactive maps you generate directly into your Drive. The exported video files can be converted into GIF files and loaded onto project sites like we see below. 

## Output Comparison

{% include gallery-figure.html img="geo_39.gif" width="100%" alt="Gif of Google Earth time lapse over Moscow, Idaho." caption="Gif of Google Earth time lapse over Moscow, Idaho demonstrating lower fidelity." %}

{% include gallery-figure.html img="geo_41.gif" width="100%" alt="Gif of ArcGIS World Imagery time lapse over Moscow, Idaho." caption="Gif of ArcGIS World Imagery time lapse over Moscow, Idaho, demonstrating improved fidelity but shortened, more contemporary time span." %}

{% include gallery-figure.html img="geo_44.gif" width="100%" alt="Gif of ArcGIS World Imagery time lapse over Moscow, Idaho." caption="Gif of Google Earth Engine time lapse over Moscow, Idaho, demonstrating improved fidelity and increased time span but difficulty of framing due to the variety of datasets." %}

