Title: What_are_Templates


Templates are files that define the user interface of your application.  In AT, contrary to server-side templating language such as JSP or ASP, the interface is processed in the browser, which eliminates round-trips to the server to fetch graphical updates.  Another great advantage of client-side templating is that you, as a developer, don't have to manually alter the display by manipulating the DOM and its CSS: all you need to do is just refresh the parts that changed when needed. This is explained in details in [the dedicated article about refresh](Refresh).

This article will show you how to write and use a template. If you are already familiar with templates and are looking for statements documentation, you'll want to read about [how to write templates](Writing Templates).

Note that templates have a specific client-side representation and that, when created, disposed or refreshed, DOM elements, DOM events and Javascript objects are manipulated. This means that depending on the structure of your application, there may be performance implications to consider (see [the disposal of a template](#Destruction)).

==Templates syntax==

To define interfaces Aria Templates relies on HTML to which it adds a number of powerful features that makes it easy to create and maintain rich front-ends.

Templates are defined in files with a <code>.tpl</code> extension and use a number of special tags identified by curly brackets <code>{</code> and <code>}</code>, which are interpreted by the AT framework engine.

There are 3 kinds of tags:
* *Statements*: like <code>{foreach}</code> or <code>{section}</code> explained in details in [this article](Writing Templates), are used to define the structure of dynamic pages.
* *Widgets*: like <code>{@aria:Button}</code> explained in details in [ this article](The Widgets Collection) are graphical components that make it easy to design a flexible user interface.
* *Expressions*: like <code>${i + 10}</code> are used to evaluate Javascript expressions (variable references, functions calls, etc.)

Templates always begin with a <code>{Template}</code> statement that defines its classpath and may provide additional configuration options.  They can contain several macros, which are the equivalent of methods, but must always contain one called <code>main</code>, that is the template entry point.
<srcinclude lang="at">templates/fibo/Fibonacci.tpl</srcinclude>

==Loading templates==

There are two ways to use templates in your application.

===The loadTemplate method===

The first one is to dynamically load them using <code>loadTemplate()</code> method of the [Aria](The Aria Singleton) global object.  This can be done anywhere in your code.  The two mandatory parameters of this method are the classpath of the template and the id of the container in which it should be loaded.

<syntaxhighlight lang="javascript">
Aria.loadTemplate({
    classpath: "ariadoc.snippets.SimpleTemplate",
    div: "output"
});
</syntaxhighlight>

This is also the method you will use in the _bootstrap code_, i.e. the container (HTML, JSP, PHP, etc.) of your application from which you will load the root template from.

<syntaxhighlight lang="html5">
<DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
    <head>
        <script type="text/javascript" src="/my-webapp/aria/aria-templates-1.1-SNAPSHOT.js"></script>
        <script type="text/javascript" src="/my-webapp/css/atdefskin-1.1-SNAPSHOT.js"></script>
    </head>
    <body>
    
    <div id="output"></div>
    
    <script type="text/javascript">
        Aria.loadTemplate({
            classpath: "ariadoc.snippets.SimpleTemplate",
            div: "output",
            data: {
                appStarted : true,
                searchEngines : ["google", "bing", "wikipedia"]
            } 
        });
    </script>
    
    </body>
</html>
</syntaxhighlight>

Note that in this example we've initialized the [data model](Data Model and Data Binding) directly at loading time using the <code>data</code> property of the configuration.  The complete list of parameters that are accepted by the <code>loadTemplate()</code> method configuration object is detailed [here](http://ariatemplates.com/api/#aria.templates.CfgBeans:LoadTemplateCfg).

===The template widget===

Another way to include a template in your application is to use the <code>{@aria:Template}</code> widget and providing it with the classpath of the file to load:

<syntaxhighlight lang="AT">
{@aria:Template {
    defaultTemplate : "amadeus.training.playground.view.OtherTemplate"
} /}
</syntaxhighlight>

Doing so will display the content of <code>OtherTemplate.tpl</code> in the template it has been included in (note that we obviously don't need to give a container id in this case.)  Similarly to the <code>loadTemplate()</code> method, it is possible to provide the template to be loaded with several other parameters such as an pre-initialized data model.  The complete list of parameters that are accepted by the Template widget is detailed [here](http://ariatemplates.com/api/#aria.widgets.CfgBeans:TemplateCfg)

==Lifecycle==

Once a template is loaded, it needs to be parsed and interpreted by the AT engine before it can be used then destroyed.  Let's have a detailed look at what happens in the different steps of a template lifecycle.

## Interpretation

After it has been loaded, the template content is interpreted by the templating engine and compiled into [a class](Aria Templates classes).  As any other class, it may contain dependencies that would be recursively loaded from the server or from the cache. Such dependencies include widgets, localized resources or [template scripts](Aria Templates Scripts).

The resulting class is kept in a cache so that it doesn't need to be reloaded and reinterpreted in future uses, making subsequent executions of this template faster.

## Initialization

If the template has been parsed and compiled successfully, the resulting class will be instantiated: its <code>constructor()</code> method, that you can overload in an associated [template script](Template Scripts), will be called.  Finally, its <code>$refresh()</code> method, inherited from <code>aria.templates.Template</code>, will be called so that it is displayed in its placeholder.

Four other inherited methods can be implemented to execute code at a specific template's phase.  Those are described in a [dedicated paragraph](Template_Scripts#Intercepting_template_lifecycle_phases) of the [template script article](Template Scripts).

## Active state

The template enters its active state once initialization is completed.  It is then visible and working.

## Destruction

A template is automatically unloaded when its parent is itself unloaded or being refreshed. The root template, loaded using <code>Aria.loadTemplate</code> on the other hand, must be manually unloaded with the <code>Aria.disposeTemplate()</code> method if needed.

When it is unloaded, the template markup is removed from the DOM and its class instance is destroyed: its <code>$destructor()</code> method is called and the object is nullified.

Remember that keeping too many references in your DOM might impact your app performances.  The whole idea behind client-side templating being to remove what is not displayed, do not forget to dispose of your templates once they are not needed anymore.