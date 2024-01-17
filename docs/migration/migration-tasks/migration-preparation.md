# Preparing Your Site for Migration

Following are the steps that must be taken to prepare your site for migration.

**This document is a work in progress, not a definitive list.**

1. Ensure your metadata is ready for migration.
    * Follow all the guidance in the [Metadata Corrections guide](/arca-docs/migration/migration-tasks/metadata-corrections/).
2. Eliminate or migrate all Orphaned Objects:
    * Orphaned objects are objects that are missing at least one of their assigned "parent" objects (newspapers, collections, etc.). Typically they become orphaned when the parent item is deleted.
    * Find a list of all your orphaned objects at `Reports -> Orphaned Islandora Objects`.
    * Look at each object in the list, and determine whether it should be kept or deleted.
        * To keep an object, `Manage` it and `Migrate` to a new parent.
        * Objects may be deleted individually, or in bulk using the checkboxes.
    * Review the Orphaned Objects report again to ensure that no new orphans were created when you deleted the last batch.
3. Eliminate or activate all your Inactive objects:
    * Go to `Islandora -> Islandora Utility Modules -> Simple Workflow Inactive Objects`
    * For each inactive object, review and determine whether it should be published or deleted.
        * To publish an object, check its box on the Simple Workflow page, and click `Publish Selected` when you have selected all the items to be published. Choose `Publish All` to publish all of them.
        * To delete an object, `Manage` the object, go to `Properties`, and choose `Permanently remove [this item] from repository`.
4. Review your XACML-restricted objects to confirm that only intended objects are restricted, and that *only Viewing restrictions are applied*:
    * Create Views that expose restricted objects:
        * Go to `Structure -> Views Import`
        * Open the following two documents, copy their contents, and paste into the Import field:
            * [Management restrictions](/arca-docs/assets/management_restrictions_import.txt)
            * [Viewing restrictions](/arca-docs/assets/viewing_restrictions_import.txt)
        * Save these new Views
    * Look at each of these views at their appropriate paths:
        * Mangement restrictions: `/restricted-objects`
        * Viewing restrictions: `/viewing-restricted-objects`
        * Export a CSV from those pages if it is easier
    * If you have objects with *Management* restrictions, remove those restrictions - Management restrictions are not useful in Islandora, and may cause problems.
    * For each object with Viewing restrictions, make sure that these are still required, and that the appropriate roles have access; if the restrictions are no longer required, remove them.
