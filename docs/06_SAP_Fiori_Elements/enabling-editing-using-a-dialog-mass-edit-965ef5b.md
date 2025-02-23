<!-- loio965ef5b2895641bc9b6cd44f1bd0eb4d -->

# Enabling Editing Using a Dialog \(Mass Edit\)

Mass editing allows users to simultaneously change multiple objects that share the same editable properties.

The values that are changed within the mass edit dialog are applied to all selected records that are editable.

> ### Note:  
> -   When the request for each selected instance is sent, the update is executed with the following logic:
> 
>     -   if some of the records fail with errors or warnings, the updates for the other selected instances are still executed
> 
>     -   individual records are only updated if all fields can be updated
> 
> 
> -   In the list report, *Mass Edit* is available for active records that don't have unsaved drafts from any user.



<a name="loio965ef5b2895641bc9b6cd44f1bd0eb4d__section_bqg_2xm_mtb"/>

## Additional Features in SAP Fiori Elements for OData V2

In SAP Fiori elements for OData V2, using the mass edit dialog is currently only available in the list report.

For more information, see [Enabling Editing Using a Dialog \(Mass Edit\) in the List Report](enabling-editing-using-a-dialog-mass-edit-in-the-list-report-7cc4f04.md).



<a name="loio965ef5b2895641bc9b6cd44f1bd0eb4d__section_mjc_ccs_hnb"/>

## Additional Features in SAP Fiori Elements for OData V4

In SAP Fiori elements for OData V4, you can use the mass edit dialog in the object page in edit mode and in the list report. Use the `enableMassEdit` property in the manifest to enable the feature. You can use the following settings for the `enableMassEdit` property:

-   Undefined \(mass edit is disabled\)

-   A Boolean property

-   An object which can contain two properties:

    -   `visibleFields`

    -   `ignoredFields`



The `visibleFields` property lets you specify which fields are available in the mass edit dialog, even if these fields are not displayed in the table. The `ignoredFields` property lets you hide specific fields from the mass edit dialog, even if these fields are displayed in the table.

You can use only either of these properties with an object. If the `visibleFields` property is set, then the `ignoredFields` property is ignored.

Both properties can contain a `FieldGroup` annotation or a comma-separated list of properties. A combination of both is not supported.

> ### Sample Code:  
> The `manifest.json` setting to toggle the mass edit functionality
> 
> ```
> "controlConfiguration": {
>    "_Item/@com.sap.vocabularies.UI.v1.LineItem": {
>       "tableSettings": {
>          ......
>          ......
>          "enableMassEdit": true
>       },
>       ......
>       ......
> }
> ```

> ### Sample Code:  
> The `manifest.json` setting to toggle the mass edit functionality using the `visibleFields` property
> 
> ```
> "controlConfiguration": {
>    "_Item/@com.sap.vocabularies.UI.v1.LineItem": {
>       "tableSettings": {
>          ......
>          ......
>          "enableMassEdit": {
>              "visibleFields": "com.sap.vocabularies.UI.v1.FieldGroup#Include"
>       },
>       ......
>       ......
> }
> ```

> ### Sample Code:  
> The `manifest.json` setting to toggle the mass edit functionality using the `ignoredFields` property
> 
> ```
> "controlConfiguration": {
>    _Item/@com.sap.vocabularies.UI.v1.LineItem: {
>       "tableSettings": {
>          ......
>          ......
>          "enableMassEdit": {
>              "ignoredFields": "property2,property3"
>       },
>       ......
>       ......
> }
> ```



### Operation Grouping for Mass Edit

You can define how the changes using mass edit should be applied:

-   Grouped for all objects within the same request \(`ChangeSet`\)

-   Separately for each object \(`Isolated`\)


You can set the mode for mass editing by using the `operationGroupingMode` key in the `manifest.json`.

> ### Sample Code:  
> `manifest.json`
> 
> ```
> "controlConfiguration": {
>    _Item/@com.sap.vocabularies.UI.v1.LineItem: {
>       "tableSettings": {
>          ......
>          ......
>          "enableMassEdit": {
>             ......
>             "operationGroupingMode": "Isolated"
>       },
>       ......
>       ......
> }
> ```

The default mode for mass editing is `ChangeSet` on the object page and `Isolated` in the list report.



### Enabling the Mass Edit Functionality

The following prerequisites apply:

-   Application developers have enabled the functionality in the manifest, as shown in the sample code above.

-   Users must select at least 1 record.

-   On the object page, *Mass Edit* is only available in edit mode.


> ### Note:  
> You can enable the mass edit feature for backend services based on CAP Node.js by setting a feature flag. For more information, see [https://cap.cloud.sap/docs/releases/dec23\#sapui5-mass-edit](https://cap.cloud.sap/docs/releases/dec23#sapui5-mass-edit).

Once a user clicks on the *Mass Edit* button, a dialog opens showing all the editable and visible fields from the table. The user is able to set a value for every editable field of the selected records, or even clear all values for the field from the selected records in one go.

The *Mass Edit* button isn't visible if the table entity has an update restriction set to static false. Visibility can also be controlled via a value for `UI.UpdateHidden` \(you can use a path-based value, but can only point to the parent path for a table on an object page or subobject page, and to a singleton in a list report table\).

The *Mass Edit* button isn't enabled if the update restriction points to a path and the path evaluates to false.

> ### Note:  
> -   *Mass Edit* doesn't change the draft or active status of the object.
> 
> -   If the parent entity has a navigation restriction to the table entity set, this takes priority over the update restriction of the table entity set.
> 
> -   If *Mass Edit* is enabled and if `selectionMode` isn't specified or set to `"Multi"`, then users can select multiple records – irrespective of other conditions \(for example, multiple selection is possible even if there are no table toolbar actions that require context\).
> 
> -   The suggestion list in the fields \(showing the values for this field from the selected records in the table\) within the *Mass Edit* dialog doesn't appear if the following applies:
> 
>     -   the field is part of the `Common.ValueListParameterIn` mapping of another field and this other field is filled with a value
> 
>         Example:
> 
>         `"Country"` has an IN mapping to `"City"`. If a user selects a value for the field `"Country"` within the *Mass Edit* dialog, no suggestion list is available for the field `"City"`.
> 
>     -   the field has a `Common.ValueListParameterOut` mapping to another field
> 
>         Example:
> 
>         If `EmployeeID` is picked from the suggestion list and if there are two reporting lines \(two different `Managers`\) for this `EmployeeID`, the OUT mapping \(the `Manager` field\) cannot be populated with the correct value.
> 
> 
> -   If *Mass Edit* is enabled for an object page and the service is ETag enabled, you must ensure that the ETag implementation is in `etag master`. This means the ETag value of the child entity doesn't depend on the ETag value of the parent \(that is, the root entity\).

> ### Restriction:  
> -   The following fields are currently not available in the *Mass Edit* dialog:
> 
>     -   date fields
> 
>     -   date-time fields
> 
>     -   image based fields
> 
>     -   progress indicator fields
> 
>     -   `DataFieldForAnnotation` based fields such as:
> 
>         -   micro chart based fields
> 
>         -   contact fields
> 
>         -   fields coming from a `FieldGroup`-based field in the table
> 
> 
>     -   multi-input fields
> 
>     -   fields coming from navigation entities
> 
>     -   custom fields
> 
>     -   fields that have context-dependent value help
> 
>         For more information, see [Field Help](field-help-a5608ea.md).
> 
>     -   value help fields with a dropdown list that have fixed values \(`@Common.ValueListWithFixedValues: true`\)
> 
> 
> -   Validation from the server for updates made in the *Mass Edit* dialog is currently not supported. There's no communication with the back end until users choose *Save* in the *Mass Edit* dialog.
> 
> -   You must not enable *Mass Edit* if the back end is configured to lock the entire object while you're making an update to a child object.

