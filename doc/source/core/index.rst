
Core
****

.. toctree::
   :glob:
   
   *


Boot structure
==============

.. parsed-literal::

   XDM.py
   main() -+
           |
         App() --+
                 |
                 +- __init__() -+
                 |              |
                 |              +-- command line options handling / parsing
                 |              +-- `xdm.init.preDB()`_: path and folder setup
                 |              +-- `xdm.init.db()`_: database initialisation and migration
                 |              +-- `xdm.init.postDB()`_: plugin loading
                 |              +-- `xdm.init.schedule()`_: recuring shedular setup and starting
                 |              +-- `xdm.init.runTasks()`_: initial (cleanup) tasks
                 |
                 +- startWebServer(): (optional)
                 |
                 |
                 +- starting of the API: (optional)
                 |
                 |
                 +- main while loop with KeyboardInterrupt handler

.. _`xdm.init.preDB()`: init.html#xdm.init.preDB
.. _`xdm.init.db()`: init.html#xdm.init.db
.. _`xdm.init.postDB()`: init.html#xdm.init.postDB
.. _`xdm.init.schedule()`: init.html#xdm.init.schedule
.. _`xdm.init.runTasks()`: init.html#xdm.init.runTasks

The common object
=================

There is a xdm.common object based on xdm.Common_ it holds following object references.


.. _xdm.Common: #xdm.Common

**Example** for ``PM``

.. code-block:: python   

   from xdm import common
   for mediaTypeManager in common.PM.getMediaTypeManager():
      print mediaTypeManager.indentifier

**instantiated during import**

- ``MM`` -- xdm.message.MessageManager_ - instance
- ``SM`` -- xdm.message.SystemMessageManager_ - instance
- ``NM`` -- xdm.news.NewsManager_ - instance
- ``SCHEDULER`` -- xdm.scheduler.Scheduler_ - instance

.. _xdm.message.MessageManager: message.html#xdm.message.MessageManager
.. _xdm.message.SystemMessageManager: message.html#xdm.message.SystemMessageManager
.. _xdm.news.NewsManager: news.html#xdm.news.NewsManager
.. _xdm.scheduler.Scheduler: scheduler.html#xdm.scheduler.Scheduler

**instantiated during boot**

- ``PM`` -- xdm.plugin.pluginManager.PluginManager_ - instance
- ``SYSTEM`` -- plugins.system.System.System_ - instance
- ``UPDATER`` -- xdm.updater.Updater_ - instance
- ``REPOMANAGER`` -- xdm.plugins.repository.RepoManager_ - instance
- ``APIKEY`` -- str

.. _xdm.plugin.pluginManager.PluginManager: ../plugin/pluginManager.html
.. _plugins.system.System.System: ../plugin/plugins/System.html
.. _xdm.updater.Updater: updater.html#Updater
.. _xdm.plugins.repository.RepoManager: ../plugin/repo.html#RepoManager

**Statuses available for Elements**
set during xdm.init_checkDefaults() (done in xdm.init.db() in App.__init__())

- ``UNKNOWN`` -- xdm.classes.Status_ - instance # no status
- ``WANTED`` -- xdm.classes.Status_ - instance # default status
- ``SNATCHED`` -- xdm.classes.Status_ - instance # well snatched and the downloader is getting it ... so we hope
- ``DOWNLOADING`` -- xdm.classes.Status_ - instance # its currently downloading woohhooo
- ``DOWNLOADED`` -- xdm.classes.Status_ - instance # no status thingy
- ``COMPLETED`` -- xdm.classes.Status_ - instance # downloaded and pp_success
- ``FAILED`` -- xdm.classes.Status_ - instance # download failed
- ``PP_FAIL`` -- xdm.classes.Status_ - instance # post processing failed
- ``DELETED`` -- xdm.classes.Status_ - instance # marked as deleted
- ``IGNORE`` -- xdm.classes.Status_ - instance # ignore this item
- ``TEMP`` -- xdm.classes.Status_ - instance # temporary item like from a search

.. _xdm.classes.Status: classes.html#xdm.classes.Status

.. note::

   Any plugin is prohibited from setting any of these attributes. At time of writing there is no measurement in place to prevent this.


.. autoclass:: xdm.Common
   :members:

