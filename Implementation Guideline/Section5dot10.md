# Long-term/fixed associations

## Example 1: Installing components/assemblies into larger items

The EPCIS Event Type `AssociationEvent` as introduced as of EPCIS 2.0 enables organisations to capture associations of physical objects that are more permanent compared to e.g. `packing` or `loading` events. This is useful to have precise visibility on which items were built into which products, assemblies, or assets.

For instance, applying Association Events is applicable in the following situations:

* installation of a sensor device into a reusable plastic tray
* construction of a rail wagon (which is made up by axles, bogies, roof components, brake systems, buffers, etc.)
* removing a (defect) component from an assembly

Prior to EPCIS 2.0, companies had to use `AggregationEvent`s for such use cases. However, the EPCIS standard allows to capture an `AggregationEvent` with an empty `childEPCs` and/or `childQuantityList` element when the `action` value is `DELETE`. Note though that plastic trays or rail waggons can also be used for more temporary aggregations such as in the course of `loading` or `packing` events. Now, if an EPCIS-based visibility system used `AggregationEvent`s also for the construction of the transport units that are part of  `loading` or `packing` events and captured an `unloading` or `unpacking` event with an empty `childEPCs` and/or `childQuantityList` element, it would mean that not just the packed or loaded objects were disaggregated from these transport units, but all the items the transport units themselves are made of, too.

For illustration purposes, presume a pool operator of reuseable plastic trays wants to properly document the installation of sensor devices (with e.g. a built-in GPS module and temperature sensor) into its trays. The reason could consist in the need to effectively identify all assets that are equipped with specific sensor devices/models in case the latter were not exactly calibrated. In addition, information on built-in sensor devices may also be enquired by customs authorities. In such a scenario, the `installing` EPCIS event could be modelled as follows:

| Dim | Data Element | V1 |
| --- | ------------ | -- |
|  | Description | Installing a sensor device in a reusable plastic tray |
|  | Event Type | Association Event |
|  | Action | ADD |
| When | `eventTime` | 12 October, 08:45 am |
| What | `parentID` | GRAI of tray |
|  | `childEPCs` | GIAI of sensor device |
| Where | `readPoint` | GLN of maintenance area |
| Why | `bizStep` | `Installing (CBV)` |

## Example 2: Installing components/assemblies into physical locations

The `AssociationEvent` is the only EPCIS event type where it is permissible to populate the `parentID` field with a physical location identifier. This feature is especially relevant for companies that need to document which particular item became an integral part of a physical location.

In a way, it is similar to the previous example, but in this situation, items are integrated into buildings or rooms rather than larger assemblies. Note that an `AssociationEvent` is not applicable if, for instance, a room is equipped with pieces of furniture - in such a case, the association is not permanent and organisations should use an `ObjectEvent` instead.

Taking the example of a company that equips a cold storage room with one or several temperature sensor devices, the corresponding EPCIS event may be modelled as follows:

| Dim | Data Element | V1 |
| --- | ------------ | -- |
|  | Description | Equipping cold storage room with temperature measuring device |
|  | Event Type | Association Event |
|  | Action | ADD |
| When | `eventTime` | 14 October, 10:55 am |
| What | `parentID` | GLN of cold storage room |
|  | `childEPCs` | GIAI(s) of sensor device(s) |
| Where | `readPoint` | GLN of warehouse location |
| Why | `bizStep` | `Installing (CBV)` |