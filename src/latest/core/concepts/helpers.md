Title: Helpers


On top of Javascript Oriented Object concepts, Aria Templates also provides you methods through helpers to achieve all the most common and repetitive tasks that you will have to face everyday.

Some of them are directly bundled within the Core layer of the framework. The other one are available in the <code>aria.utils</code> package, ready for you to use them as a [dependency](Around_Classes#Dependencies.2C_.24dependencies) inside your classes

Here is a non exhaustive list of what is available:
* [Array](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Array), [Object](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Object), [Type](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Type)
* [Date](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Date)
* [Math](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Math), [Random](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Rand) and [Number](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Number)
* [Json](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json)
* [Dom](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Dom)
* and many more you will discover by yourself...

Let's focus in this article on the 2 most important one, or at least, the 2 one you will have to use daily

# Ajax Helper

The Ajax Helper is one the helpers which is directly embedded inside the Core Layer. It's available for you to use through the [aria.core.IO](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.core.IO) singleton class.

You can easily declared and execute an XHR call in your class within just a few lines of code.

The usage of this helper is mainly described in the [Working in an Asynchronous World](Working_in_an_Asynchronous_World) article. If not already done, we strongly encouraged you to read it.

# JSON Helper

Working with Aria Templates means to manipulate a lot of JSON objects, and various data. This utility helper is designed to help you manage these sometimes complex manipulations.

Using this helper, you basically can:
* Serialize / Deserialize Json data using the 2 methods <code>[convertToJsonString()](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json:convertToJsonString:method)</code> and <code>[load()](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json:load:method)</code>

<srcinclude tag="serialization" lang="Javascript" outdent="true">core\helpers\JsonManipulation.js</srcinclude>

* Performs some tests over your objects, like does this object contains this key with <code>[contains()](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json:contains:method)</code>, or test is a particular node is a metadata node with <code>[isMetadata()](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json:isMetadata:method)</code>. You can also create a fresh new clone of an existing object using <code>[copy()](http://www.ariatemplates.com/aria/guide/apps/apidocs#aria.utils.Json:copy:method)</code>
<srcinclude tag="standard" lang="Javascript" outdent="true">core\helpers\JsonManipulation.js</srcinclude>

* Apply the Observable pattern to any node of any of your Json object. What does it mean exactly ? It means that you can add listeners to your Json objects in order to be called back anytime a change is done to your data.
<srcinclude tag="listeners" lang="Javascript" outdent="true">core\helpers\JsonManipulation.js</srcinclude>

Again, as you can see, the second argument of the <code>aria.utils.Json.addListener()</code> method is an Aria Templates callback object. Once inside your callback, you will be provided as argument an object that contains few properties:
* *dataHolder*: the object containing the property that has been changed
* *dataName*: the property name on the object
* *oldValue*: the previous value
* *newValue*: the new value