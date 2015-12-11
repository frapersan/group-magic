# Introduction #

Group-Magic is an external enrollment plugin for [Moodle 2.0](http://moodle.com) that was created to remove the need to manually group students who have enrolled in courses. The plugin automatically places students into groups after enrolling them in a course. If a group does not exist, a new group is automatically created and the student is added to the new group.


# Details #

The following feature settings can be configured via  Moodle's [plugin management interface](Installation#Plugin_Management.md):

## Auto-group maximum group size (Integer) ##
The plugin will not fill groups which exceed the maximum group size. If no group exists with a size below the maximum size, a new group will be created to add new students. The plugin will **not** remove users from any group that exceeds the maximum group size, so manual overloading of groups is possible.

## Auto-fill priority (Sequential or Balanced) ##
Determines the method for filling all existing groups which are below the maximum group size. The Sequential fill setting will add students to largest group. The Balanced fill setting will add students to the smallest group.

## Create new groups (Yes or No) ##
If enabled, a new group will be created automatically by the plugin if all existing groups are full. When disabled, the plugin will not create new groups, but this means that new students will not be placed into a group.

## Use database transaction (Optional) ##
If enabled, the plugin will attempt to use Moodle's database transaction when adding a student to a group.