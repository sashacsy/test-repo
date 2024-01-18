# Preparing Your Site for Migration

Following are the steps that must be taken to prepare your site for migration.

**This document is a work in progress, not a definitive list.**

## Metadata Corrections

Ensure your metadata is ready for migration.

  * Follow all the guidance in the [Metadata Corrections guide](/arca-docs/migration/migration-tasks/metadata-corrections/).

## Remove orphaned objects

Eliminate or migrate all Orphaned Objects:

  * Orphaned objects are objects that are missing at least one of their assigned "parent" objects (newspapers, collections, etc.). Typically they become orphaned when the parent item is deleted.
  * Find a list of all your orphaned objects at `Reports -> Orphaned Islandora Objects`.
  * Look at each object in the list, and determine whether it should be kept or deleted.
      * To keep an object, `Manage` it and `Migrate` to a new parent.
      * Objects may be deleted individually, or in bulk using the checkboxes.
  * Review the Orphaned Objects report again to ensure that no new orphans were created when you deleted the last batch.
  * *No orphaned objects should remain in the repository.*

## Inactive Objects

Eliminate or publish all your Inactive objects:

    * Go to `Islandora -> Islandora Utility Modules -> Simple Workflow Inactive Objects`
    * For each inactive object, review and determine whether it should be published or deleted.
        * To publish an object, check its box on the Simple Workflow page, and click `Publish Selected` when you have selected all the items to be published. Choose `Publish All` to publish all of them.
        * To delete an object, `Manage` the object, go to `Properties`, and choose `Permanently remove [this item] from repository`.
*No inactive object should remain in the repository.*

## Review XACML policies

Review your XACML-restricted objects to confirm that only intended objects are restricted, and that *only Viewing restrictions are applied*:

  * Create Views that expose restricted objects. This will be done twice; once for a Management Restrictions view, and again for a Viewing Restrictions view.
      * Go to `Structure -> Views Import`
      * Open the relevant Views Import document, copy its contents, and paste into the Import field:
          * [Management restrictions](/arca-docs/assets/management_restrictions_import.txt)
          * [Viewing restrictions](/arca-docs/assets/viewing_restrictions_import.txt)
      * Save your new View
  * Look at each of these views at their appropriate paths:
      * Mangement restrictions: `/restricted-objects`
      * Viewing restrictions: `/viewing-restricted-objects`
      * Export a CSV from those pages if it is easier
  * If you have objects with *Management* restrictions, remove those restrictions - Management restrictions are not useful in Islandora, and may cause problems.
  * For each object with Viewing restrictions, make sure that these are still required, and that the appropriate roles have access; if the restrictions are no longer required, remove them.

## Remove "deleted" objects

Some objects may have the "deleted" property set, but have not yet been removed from the repository. Identify, review, and remove all "deleted" objects.

  * Go to `Islandora -> Manage Deleted Objects`
  * Select all content models, to see all "deleted" objects
  * Review the objects, and either restore or purge them. No "deleted" objects should remain in the repository.

## Review Users list

Review your list of users at `/admin/people`:

  * If any users should no longer be in the system, cancel their accounts.
  * If any users have the wrong role, edit the user and change them.
  
    
[Contact the Arca Office](mailto:arcaoffice@bceln.ca) if you need any assistance with these tasks.