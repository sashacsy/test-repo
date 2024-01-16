# TASK: Metadata Corrections

In preparation for migration, all Arca Admins are asked to ensure that their metadata is correct, to standard, and ready to migrate. While all metadata should be consistent and good-quality, there are certain key elements that must be addressed before a site can be migrated.

[Facet pages have been set up](https://arcabc.ca/browse/genre) to expose the values of most key metadata fields. You can see your site's fields particularly at the same path: `/browse/genre`. Please [mailto:arcaoffice@bceln.ca](contact the admin centre) for help setting up your facet pages.

When your metadata has been corrected, please be sure to apply those standards to any objects you ingest going forward, so that correction work does not need to be done again.

## Key metadata fields and how to handle them

* [Dates](https://arcabc.ca/browse/date):
    * Must be formatted to the [W3C Standard](https://www.w3.org/TR/NOTE-datetime). This means:
        * Numerical format only (no text)
        * YYYY-MM-DD, or YYYY-MM if only year and month are known, or YYYY if only the year is known.
    * If your date cannot meet the W3C standard (e.g. completely unknown, or certain digits are unknown), we can accommodate [EDTF](https://www.loc.gov/standards/datetime/). 
        * Some EDTF examples:
            * | **EDTF Format** | **Use for**                                       |
            | --------------- | ------------------------------------------------- |
            | 1933?           | 1933 (year uncertain)                             |
            | 1945~           | 1945 (year approximate)                           |
            | 2016-04-12      | 2016-04-12                                        |
            | 1860/1880?      | 1860 to 1880 (year uncertain)                     |
            | 1870/1880       | 1870 to 1880                                      |
            | 19XX            | Sometime in the 1900s (decade and year uncertain) |
            | 19X3            | Decade uncertain, year known                      |
            | XXXX-12-XX      | Some day in December, unknown year                |
* [Genres](https://arcabc.ca/browse/genre):
    * Terms must be from either the [MARC Genre Terms list](https://www.loc.gov/standards/valuelist/marcgt.html), or the [Art & Architecture Thesaurus](https://www.getty.edu/research/tools/vocabularies/aat/).
        * Include the authority used in your MODS, e.g. `<genre authority="marcgt">book</genre>`, or `<genre authority="aat">comic operas</genre>`.
        * Follow the exact format the authority uses for the term: **all lowercase, no punctuation**; AAT is generally plural, MARCGT singular. 
    * If it is truly impossible to find a term in either of the above vocabularies, use the `authority="local"` attribute.
        * Check the standard vocabularies for a synonym first.
        * Format your term to match existing usage (i.e. lowercase, no punctuation).
    * There should be *only one value per `<genre>` element*. If your object has more than one genre, use multiple genre elements -- i.e. `<genre authority="marcgt">thesis</genre>  <genre authority="aat">photography</genre>`
* Names ([personal](https://arcabc.ca/browse/names_personal) and [corporate](https://arcabc.ca/browse/names_corporate)): 
    * Names must be consistent and deduplicated.
        * For example: if "Weigel, Brandon"; "B. Weigel", and "Brandon John Weigel" appear in your repository, and they are all the same person, those values *must be identical* across all your metadata. Choose one form of the name that is used in all of your objects.
    * Ensure that corporate vs personal names are properly tagged.
        * i.e. if "ABC Company" shows up under `<name type="personal">`, that should be corrected to `<name type="corporate">`.
    * Ensure that the form of name you choose is the one you wish to apply for all instances of that name going forward, as each name will become an entity in your repository.
*  [Role terms](https://arcabc.ca/browse/roles):
    * All role terms should match the [MARC Relator Term List](https://www.loc.gov/marc/relators/relaterm.html).
        * Ensure consistency in spelling, format, and case.
    * If your name has more than one role associated with them, use multiple `<role>` element sets. i.e.
        ```<name type="personal">
             <namePart>Weigel, Brandon</namePart>
             <role>
               <roleTerm authority="marcrelator>">Director</roleTerm>
              </role>
              <role>
                <roleTerm authority="marcrelator>>Producer</roleTerm>
              </role>
          </name>```  
    * If there is no MARC Relator term that fits your person's role, you may use a local term with `authority="local"` noted.
        * Be consistent with your local terms: i.e. consistent case, consistent spelling, etc.
*  [Type of Resource](https://arcabc.ca/browse/typeofresource)
    * Values in the MODS element `<typeOfResource>` may **only** come from the following list:
        * cartographic
        * manuscript
        * map
        * mixed material
        * moving image
        * notated music
        * software, multimedia
        * sound recording
        * sound recording-musical
        * sound recording-nonmusical
        * still image
        * text
        * three dimensional object
* Subjects - includes [topical](https://arcabc.ca/browse/subject), [geographic](https://arcabc.ca/browse/geographic), temporal, and named subjects):
    * Subject terms are not controlled throughout Arca, but should be consistent within your own repository.
    * Review your subjects to make sure they are deduplicated, consistent, and broad enough to group like objects together.
    * Multiple subjects must be in separate elements. 
        * Example: an object with the subjects "Fitness, Bowling, and Recreation" will look like:
          ```
          <subject>
            <topic>Fitness</topic>
          </subject>
          <subject>
            <topic>Bowling</topic>
          </subject>
          <subject>
            <topic>Recreation</topic>
          </subject>
          ```
    * For named subjects, follow the same standards as you apply to names.

These are the most critical metadata elements to correct, but best practice is to review all of your more common metadata fields to ensure consistency and accuracy before we migrate your data.

## Why must we do this?

In Islandora 7, MODS metadata is free-form, and objects are more or less independent of one another. In Islandora 2, the system is better-integrated, and our metadata schema uses **term references** for certain fields.

Metadata like Genres, Roles, Names, and Subjects are no longer plain text -- they are Taxonomy Term References. This means that they are stored separately in your database, and linked to. The benefit of this is that any time you have an object by "B. Weigel", instead of typing "B. Weigel" or "Brandon Weigel" or another form in the Name field, you select the canonical name from a list, and create a link to the "B. Weigel" entity. Same with Genre, Role, and Subject.

In order for our migrated metadata to be accurate, those terms and names must be correct and consistent across *all* Islandora objects. Each different value under `<genre>` will create a separate taxonomy entry -- it will be bad for your repository to have "Book", "book", "books", and "Books." as different options with different objects collected under them.

Similarly, you don't want your repository collecting some works under "B. Weigel" and others under "Brandon Weigel" if they all belong to the same author.

This cleanup task must be done *before* the migration, because the creation of those terms is automated. And it is easier to correct text pre-migration than to merge terms and correct database entires afterward.

## How do I create my own Facet Pages?

* Enable the module "Islandora Solr Facet Pages"
* Configure the module under `Islandora -> Solr -> Facet pages`:
    * For each field you want to browse, enter the Solr field in question (e.g. `mods_genre_ms`), a readable label, and a path (e.g. `genre`)
        *  To identify the Solr fields you're interested in, see the [Useful Solr Fields](https://docs.google.com/document/d/1SEU5nxGPJNXbQx3767Y4YoVzOSvoPtHHWO8Ux8rbllU/edit?usp=sharing) document.
* Under `Structure -> Blocks`, enable the block `Islandora Solr Facet Pages` under the `First Sidebar` region.
    * Configure the block, to appear under "Only the listed pages" and enter `browse/*`.
* Navigate to your facet pages by going to `/browse/[path]`. In the `genre` example given above, that would be `/browse/genre`.    

## How do I make corrections?

The best approach to batch metadata changes is using the module Islandora Datastreams I/O, and a text editor like BBEdit or Notepad++. This video walks you through the process: https://youtu.be/hjBRml74_eY

The modules that showed you the field values will also link you out to search results showing all of the objects with a given value. From those results, you can collect a list of PIDs or copy the Solr search to use with Datastreams I/O to export the MODS for editing.

If you have difficulty or are not comfortable with the process, please [let the Arca Office know](mailto:arcaoffice@bceln.ca)! We will be more than willing to help you out.
   