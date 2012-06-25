Title: TabPanel



The content of the Tab widget is presented by using TabPanel widget. The TabPanel widget loads the content in one of 2 ways. The method depends on whether the TabPanel has been used as a container or not. If it has been used as a container, then the details for which data should be displayed need to be described in the TabPanel itself.

If the TabPanel hasn't been used as a container then it is necessary to specify a macro, which is used in the refresh. This method is considerable faster and is recommended where possible.

Here is an example of initialising a TabPanel widget
<srcinclude tag="wgtTabPanelSnippet1" lang="AT" outdent="true">widgets/tabpanel/Snippet.tpl</srcinclude>

The whole list of configuration parameters is available in [TabPanelCfg bean](http://ariatemplates.com/aria/guide/apps/apidocs/#aria.widgets.CfgBeans:TabPanelCfg).

Since TabPanel widget is used along with Tab widget, for more understanding please read [Tab Widget](Tab).

Here is an example for TabPanel widget using macro content

<sample sample="widgets/tab/tabpanel" />