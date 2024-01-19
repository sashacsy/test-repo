# Troubleshooting in Islandora 7

Troubleshooting documentation is currently scattered around https://arca.bcelnapps.ca.
    
!!! warning
    This document will be an attempt to consolidate that information. It will be obsolete after the migration.

## Legacy FAQs
### Display
<details>
<summary>How can I change the repository name in my breadcrumbs?</summary>
<br>

To batch export datastreams for newspaper issues, use the Islandora Datastreams I/O (information in [this Arca Minute](https://youtu.be/hjBRml74_eY)). Select the Solr Query option and enter the following text: `RELS_EXT_isMemberOf_uri_mt:"PID"` (replacing `"PID"` with the PID for the newspaper collection). This will export all of the child objects for that newspaper. For example, to make batch metadata edits, select the MODS datastream.
  
</details>
<details>
<summary>How do I control how my collections are sorted?</summary>
<br>

You can use Solr fields to determine how your collections' objects are sorted.

1. Make sure that your Collection Solution Pack is configured to use Solr for display generation `(admin/islandora/solution_pack_config/basic_collection)`
2. Under “Sort field for collection query”, enter your chosen field followed by `asc` or `desc`. For dates, use the `_dt` version of the field. For other field types, use `_ss`.
    - Use `mods_titleInfo_title_ss asc` to sort by title A-Z.
    - Use `mods_originInfo_dateIssued_dt desc` to sort by date, newest to oldest.
3. If you want to be able to configure different sort strings for individual collections, check “Allow individual sort strings per collection”
    1. Manage the collection that you want to sort differently
    2. On the Collection tab, choose Set Solr Sort String
    3. Enter a sortable string (ends with `_ss`) followed by `asc` or `desc` to use for sorting this collection.
4. If you want to use the Islandora Sort block to allow multiple sort fields, enable it at Structure -> Blocks. Choose the sort fields under "Sort Settings" in the Solr configuration screen (admin/islandora/search/islandora_solr/settings).
</details>
<details>
<summary>Metadata display changes for Citation/Thesis objects don't take effect</summary>
<br>

If you're making changes to you Citation or Thesis objects' metadata display profiles (at Islandora -> Solr Index -> Metadata display), check our Scholar configuration.  
  
Go to Islandora -> Solution Pack Configuration -> Scholar. Scroll down to find the option "Use Standard Metadata Display". Make sure it is checked.  
  
Scholar defaults to its own metadata display, [COinS](https://en.wikipedia.org/wiki/COinS). This display is not configurable, and so is probably not the best choice for objects in Arca.
</details>
<details>
<summary>One of my display fields shows multiple values instead of just one. How do I fix it?</summary>
<br>

When viewing an object, one or more of the metadata display fields shows several values in it, from several different elements that were added separately to the ingest form. How can I display one specific value?  
  
Your metadata display profile (or Solr search result field, or wherever you're using a Solr field) is probalby using the Dublin Core Solr field (`dc.description`, `dc.identifier`, etc.) rather than the more granular MODS Solr field. Dublin Core is not as precise as MODS, and many DC elements combine different MODS elements into one.  
  
For example, dc.description will combine MODS abstract, and all MODS note elements. If you want to display just one of these, choose the MODS field that contains the specific piece of metadata you require.
</details>
<details>
<summary>My object doesn't display the fields I want. Do I need to change the ingest form?</summary>
<br>

No!  
  
Your ingest form is probably fine. Solr generates many fields based on the metadata you ingest via the form, and your object's content model metadata display profile is only configured to display a certain selection of those. You can configure the fields used in your Solr Metadata Display in admin/islandora/search/islandora_solr/metadata.  
  
For detailed assistance with this, including ways to find out which fields you should be using, review the [Metadata Display Arca Hour](http://ac-connect.bccampus.ca/p34d7sm0zb7/).
</details>

### Enhancements

<details>
<summary>Some objects are not downloadable by end users. How can I change this?</summary>
<br>

While some Islandora content models provide a download link on the object view page, others do not. This requires one or more extra steps.  
  
First, enable the Islandora Downloadable Datastreams module, and configure it under Islandora -> Islandora Utility Modules -> Islandora Downloadable Datastreams. You will need to select the types of objects for which to generate a download link.  
  
Then, enable the Download block and place it in the appropriate page area (Structure -> Blocks).  
  
If your object still does not have a download link, check to make sure that the appropriate datastream is actually present (Manage -> Datastreams).  
  
For Book and Newspaper Issue objects, you will need to generate a PDF datastream first. Manage the object. Find the "Create PDF" button: For Books, it's under the Book tab. For Newspaper Issues, it's under the "Issue" tab.  
  
When the PDF datastream has been created, the download block will appear when you view the object.
</details>
<details>
<summary>Can Arca provide links to fulltext articles outside the repository?</summary>
<br>

If I ingest a Citation or Thesis object but I don't have a PDF that I can include, how can I direct users to access a fulltext version of the article?  
  
If your objects contain a DOI in a MODS Identifier field `<identifier type = "doi">`, several options become possible. The DOI itself can be linked to externally-hosted articles, but often those articles are behind paywalls - even when open-access versions exist.  
  
To get around this, the [Islandora Badges](https://github.com/bondjimbond/islandora_badges) module has a submodule called [Islandora oaDOI](https://github.com/bondjimbond/islandora_badges/tree/7.x/modules/islandora_oadoi).  
  
Islandora queries the [oaDOI.org](https://oadoi.org/) service, which provides links to free, open-access fulltext versions of articles with DOIs, if they exist. The module creates a block which appears on any objects that (a) do not have a PDF datastrea, and (b) do have a free fulltext link available through oadoi.org, and offers a link for the user to access the fulltext externally.  
  
To use Islandora oaDOI: 
1. Go to Modules (admin/modules), and enable (1) Islandora Badges, and (2) Islandora oaDOI. 
2. Configure Islandora Badges (under Islandora Utility modules, or admin/islandora/tools/badges), and make sure that the Thesis and Citation content models are selected.
3. In the Blocks menu (admin/structure/block), find the block titled "Islandora oaDOI Link" and add it to the region of your choice. (Recommended: Content or Sidebar Second.)
4. Use CSS Injector to style the block as you desire.
</details>
<details>
<summary>Can I integrate LDAP into my Arca instance?</summary>
<br>

An SSL certificate is a prerequisite for LDAP integration, and there may be a cost to implement and maintain it. Let the [Admin Centre](http://bceln.ca/contact/-arca-administrative-centre#profile-contact) know if you are interested in an LDAP integration and we’ll talk next steps.
</details>

### Managing Objects

<details>
<summary>How do I batch export datastreams for newspaper issues?</summary>
<br>

To batch export datastreams for newspaper issues, use the Islandora Datastreams I/O (information in [this Arca Minute](https://youtu.be/hjBRml74_eY)). Select the Solr Query option and enter the following text: `RELS_EXT_isMemberOf_uri_mt:"PID"` (replacing `"PID"` with the PID for the newspaper collection). This will export all of the child objects for that newspaper. For example, to make batch metadata edits, select the MODS datastream.
</details>

<details>
<summary>How can I delete/migrate/share lots of objects at once?</summary>
<br>

When managing a collection, the options on the Collection tab let you manage multiple objects at once. These views by default only shows 10 objects at a time. For 1,000+ objects, this means a lot of screens and a lot of clicking.  
  
But the number of objects that appear on this screen can be configured. Just go to the Collection Solution Pack config page: admin/islandora/solution_pack_config/basic_collection  
  
Under the field “Objects per page during collection management”, just change the number. 
</details>
<details>
<summary>How do I change how Book pages are displayed?</summary>
<br>

By default, the Book Viewer makes several assumptions about Book objects:
- That you want to display two pages side by side (or whatever default you set in the general Book configuration)
- That your book has a cover, and so presents the first page alone and subsequent pages side by side
- That your pages were ingested in the correct order
- That your book reads right-to-left  
  
Each of these things can be configured by Managing the book and clicking to the Book tab.
- Page Progression: choose between right-to-left and left-to-right
- Page Display Mode: choose to display pages one at a time, or two side by side
- Book Cover: Indicate whether your book has a cover. This toggles whether Page 1 is shown alone or beside Page 2.
- Reorder Pages: Rearrange individual pages.
- Delete pages: Delete individual pages.
</details>
<details>
<summary>How do I delete an object?</summary>
<br>

There are two ways to delete objects. The preferred method is by managing the object itself. The less-preferred method is to do so by managing the object's parent collection (or parent newspaper/book).  
  
#### Preferred method  
1. Manage the object (navigate to the object and click the Manage tab)  
2. Go to Properties
3. Click "Permanently remove [this object] from repository"
    - If the object is a collection, it will instead say "Delete collection".  
  
The object and any of its children (if applicable) will be removed.  
  
 #### Less preferred method  
Another way to delete an object is to do so via the Collection tab when managing its parent collection. If the object you're deleting has children (e.g. if it is a newspaper issue, book, compound object, collection, etc.), those children will not be deleted.
  
1. Manage the parent collection/newspaper issue/etc.
2. Click the Collection tab if you are managing a collection, or the relevant tab for the content model you are working in
3. Click the "Delete members" section
4. Check off the objects you want to delete, and click "Delete selected"
5. If the objects deleted had any children, check the Orphaned Objects page (Islandora -> Islandora Utility Modules -> Orphaned Objects) to view and delete any orphans you may have created.  

#### Deleting newspapers  
We recently discovered [a bug in Islandora's handling of newspapers](https://jira.duraspace.org/browse/ISLANDORA-2050): even if you delete the Newspaper via its Properties screen, the Pages of its Issues will become orphans. Please use the Islandora Orphaned Objects module to purge any orphans you might have created after deleting a newspaper.
<details>

### Statistics

<details>
<summary>How can I access usage stats for my objects?</summary>
<br>

There are several ways, depending on what you need.  
  
- For a complete picture of your visitors' usage, ask the Admin Centre for a Piwik account. This will give you access to your own dashboard at https://analytics.bceln.ca, where you will be able to access all sorts of data and views on that data.
- For views and downloads on any given object:
   - Enable the Islandora Usage Stats Reports module.
   - Under Structure -> Blocks, enable the Object Usage block and choose a region to display it.
   - When an object is viewed, it will now display a count of all views and all downloads since the object was ingested.  
  
- For a complete list of datastream downloads per object:
   - In the Modules menu, enable "Islandora Datastream Downloads Report". (You may need to flush the cache afterward.)
   - This creates a view (simply called "Downloads" in the Views menu) and a page at yoursite.arcabc.ca/download_stats.
   - On this page, you'll get a list of objects and a record of each download of each datastream. You can also download a CSV file of all these records, which you can manipulate in Excel to filter by date, etc.
   - Date filtering on the webpage is not working right now; this is an improvement we will work on.
</details>
