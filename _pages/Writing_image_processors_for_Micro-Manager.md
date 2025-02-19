---
autogenerated: true
title: Writing image processors for Micro-Manager
redirect_from: /wiki/Writing_image_processors_for_Micro-Manager
layout: page
---

Micro-Manager has an image processing pipeline. Every time the program
receives an image from a camera, it is run through this pipeline, and
the DataProcessors in the pipeline make modifications to the image. For
example, DataProcessors can:

1.  Rotate and mirror images so that they have the same orientation as
    images from other cameras or as the microscope eyepiece
2.  Apply offset/gain correction
3.  Generate overlaid color images for cameras that split their sensor
    across two different dichroics

et cetera. An image goes in, the DataProcessor does some work, a
different image comes out.

New DataProcessors must implement the [`DataProcessor`
interface](https://valelab.ucsf.edu/trac/micromanager/browser/mmstudio/src/org/micromanager/api/DataProcessor.java).
This interface specifies three functions: process(),
makeConfigurationGUI(), and dispose(). The first function is what does
the actual modification of image data (and is required to be
implemented); the latter two deal with creation of user interfaces for
configuring the processor, and their destruction, respectively. They are
optional, but if you do not implement them then your DataProcessor
cannot be configured at runtime.

You can see and adjust the current image pipeline by going to the Tools
-&gt; Image Pipeline... menu option. From this dialog, all
DataProcessors that Micro-Manager knows about can be seen, created, and
re-ordered. To add a DataProcessor to this list, it must be
*registered*.

There are two primary ways to register a new DataProcessor. The first is
to call the `registerProcessorClass` function on the [`gui`
object](https://valelab.ucsf.edu/trac/micromanager/browser/mmstudio/src/org/micromanager/api/ScriptInterface.java).
This function takes as arguments the "class" of your DataProcessor and a
`String` that names it (it is this string that is displayed in the Image
Pipeline's list of known processors). The class is what you get if you
access the `.class` field of your processor. For example, let's say you
have a DataProcessor named MyProcessor:

```
import mmcorej.TaggedImage;

import org.micromanager.api.DataProcessor;

import org.micromanager.acquisition.TaggedImageQueue;

import org.micromanager.util.ReportingUtils;

class MyProcessor implements DataProcessor {

```
  public void process() {
     try {
        TaggedImage image = poll();
        // "Poison" means the queue is empty. We just pass the poison along
        // the queue in that case. It's not a valid image so don't try to 
        // process it.
        if (image != TaggedImageQueue.POISON) {
           // ... modify the image here ...
        }
        produce(image);
     }
     catch (Exception ex) {
        // Something went wrong; at bare minimum we should log the error.
        ReportingUtils.logError(ex);
     }  
  }
```

}
```

This particular DataProcessor does not implement makeConfigurationGUI()
and dispose(), ergo it is not configurable. To register this
DataProcessor, first you should compile it and put the `.class` file in
the *mmplugins* directory. Then, in your code, you would do this:

```
gui.registerProcessorClass(MyProcessor.class, "My Processor");
```

Once this is done, the new DataProcessor will show up in the Image
Pipeline dialog, where the user can add and remove it from the pipeline.

Alternately, you can create an [`MMProcessorPlugin`
object](https://valelab.ucsf.edu/trac/micromanager/browser/mmstudio/src/org/micromanager/api/MMProcessorPlugin.java).
These are plugins that specifically create DataProcessors when selected
from Micro-Manager's "Plugins" menu. MMProcessorPlugins are similar to
MMPlugins (they both inherit from the same [`MMBasePlugin`
interface](https://valelab.ucsf.edu/trac/micromanager/browser/mmstudio/src/org/micromanager/api/MMBasePlugin.java)).
However, because the DataProcessor is responsible for providing its own
GUI via `makeConfigurationGUI`, MMProcessorPlugins do not have the
`show()` and `dispose()` methods that MMPlugins do. Instead, they must
provide a `getProcessorClass` function (in addition to the functions and
fields required by MMBasePlugin). For example:

```
import org.micromanager.api.MMProcessorPlugin

class MyProcessorPlugin implements MMProcessorPlugin {

  // How this plugin will be shown in the "Plugins" menu. 
  public String menuName = "My Processor";
  // Optional extra tooltip string that will be shown when the mouse hovers
  // over this plugin in the "Plugins" menu.
  public String tooltipDescription = "My custom plugin for processing images";

  public static Class<?> getProcessorClass() {
     return MyProcessor.class;
  }  

}
```

When Micro-Manager starts up, it automatically loads all of its plugins.
At this point, it will detect if your plugin is an MMProcessorPlugin,
and if so, it will automatically register the corresponding
DataProcessor so that it can be created. Additionally, when your plugin
is selected from the "Plugins" menu, Micro-Manager will:

1.  Create a new DataProcessor if one does not already exist.
2.  Add that DataProcessor to the image pipeline.
3.  Display the configuration GUI for that processor.
