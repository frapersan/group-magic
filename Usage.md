# Basic Usage #

## Constructor and Options ##

Constructor: create instance of the GroupMagic class using an options array.

```
$gm = new GroupMagic( $options );
```

Option list.

| Property | Description | Type |
|:---------|:------------|:-----|
| maxGroupSize | maximum number of users to place in a group | int  |
| groupFillType  | method for filling groups | 'seq' or 'para' |
| createGroups | Automatically create groups as needed | boolean |
| useDbTransaction | Use a database transaction when adding user to a group | boolean |
| autoGroupRole | User role in course to auto-group | int  |

```
$gm = new GroupMagic( array(
    'maxGroupSize' => 25,
    'groupFillType' => 'seq',
    'createGroups' => true,
    'useDbTransaction' => true,
    'autoGroupRole' => 5 // 5 = student role id
) );
```

## Auto-group method ##

```
$gm->addUserToCourseGroups( $userId, $courseId, $roles );
```

Parameters

| Parameter | Description | Type |
|:----------|:------------|:-----|
| userId    | User id     | int  |
| courseId  | Course id   | int  |
| roles     | List of role ids associated with given user and course | array() |

# Examples #

## Constructor and options ##

Automatic grouping of users which are students for a given course.
Fill groups sequentially up to a maximum size of 25 users.
Create new groups as needed.

| Property | Value | Description |
|:---------|:------|:------------|
| maxGroupSize | 25    | maximum number of users in a group |
| autoGroupRole | 5     | 5 = student role in Moodle 2.0 |

```
$gm = new GroupMagic( array(
    'maxGroupSize'   => 25,
    'autoGroupRole'  => 5 // student
) );		
```

## Auto-group a user enrolled in a course ##

For a given user enrolled in a course with the role of student and guest.
| Property | Value | Description |
|:---------|:------|:------------|
| userId   | 25    | example user id - hypothetical |
| courseId | 124   | example course id - hypothetical |
| roles    | array( 5, 6 ) | list of roles for user enrolled in course - hypothetical |

```
$gm->addUserToCourseGroups( 25, 124, array( 5, 6 ) );
```

## Example use in enrollment plugin ##

Use in sync\_user\_enrolments() function after updating enrollment for a user.

```
public function sync_user_enrolments( $user ) {

    // GroupMagic options can be added to the plugin configuration
    // and retrieved via the get_config() function

    $gm = new GroupMagic( array(
        'maxGroupSize' => $this->get_config('maxgroupsize'), 
        'groupFillType' => $this->get_config('autofilltype'), 
        'createGroups' => $this->get_config('createnewgroups'), 
        'autoGroupRole' => $this->get_config('defaultrole') 
    ) );


   $enrols = array();

   /* 
        ... plugin enrollment code ...
        ... store enrollment data for user in array as below:

        $enrols = array( 
            [course id 1] => array( role id 1, role id 2, ... ),
            [course id 2] => array( role id 1, ... ),
            ...
        );
    */

    foreach( $enrols as $courseId => $roles ) {
        $gm->addUserToCourseGroups( $user->id, $courseId, $roles );
    }
}
```