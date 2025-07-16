## How to write presets

The process of writing presets is pretty straightforward, but there are some fundamental concepts that should be understood before attempting to write your own presets.

Fundamentally, a preset is just a rendered CLI snippet. The configurator has a presets engine that is designed to parse a presets file (which can be found in the `presets` folder), and provide an interface to users that allows for selection of options.

Presets have metadata that are required to be set. The following table goes over the various meta tags and gives a brief description and example of them. All tags are prefixed with `#$` to indicate to the parser that the data is not to be included in the final rendered CLI snippet.

### Required Tags

| Tag | Description | Example |
|-----|-------------|---------|
| `TITLE` | Preset Display Title | `#$ TITLE: Example Preset` |
| `DESCRIPTION` | Preset description | `#$ DESCRIPTION: Changes the utilized RPM port` |
| `AUTHOR` | Preset Author | `#$ AUTHOR: FlyDragonRC` |
| `FIRMWARE_VERSION` | Firmware Version(s) that are compatible with the preset | `#$ FIRMWARE_VERSION: 4.4` |
| `CATEGORY` | Preset category | `#$ CATEGORY: REMAPPING` |
| `STATUS` | Preset status | `#$ STATUS: COMMUNITY` | 

### Optional Tags
| Tag | Description | Example |
|-----|-------------|---------|
| `BOARD_NAME` | Board name(s) that are compatible with the preset | `#$ BOARD_NAME: FLYDRAGONF722_V2` |
| `INCLUDE` | Preset content inclusion | `#$ INCLUDE: path/to/included/preset/warning.include` | 
| `KEYWORDS` | Preset keywords (for search) | `#$ KEYWORDS: rpm-s rpm-e rpm` |
| `DISCUSSION` | Preset discussion link | `#$ DISCUSSION: ` |
| `WARNING` | Preset warning | `#$ WARNING: This preset is an example preset and should not actually be used!` |
| `INCLUDE_WARNING` | Preset warning inclusion  | `#$ INCLUDE_WARNING: path/to/included/warning.include` |
| `INCLUDE_DISCLAIMER` | Preset disclaimer inclusion | `#$ INCLUDE_DISCLAIMER: path/to/included/disclaimer.include` |
| `PRIORITY` | Preset **display** priority | `#$ PRIORITY: 150` |
| `FORCE_OPTIONS_REVIEW` | Display a warning prompt about options review | `#$ FORCE_OPTIONS_REVIEW: true` |
| `PARSER` | Preset description parser | `#$ PRIORITY: 0` |
| `OPTION_GROUP BEGIN`/`OPTION_GROUP END` | Declare a option group | See tag documentation |
| `OPTION BEGIN`/`OPTION END` | Declare an option | See tag documentation |

### Tag Documentation

#### `TITLE`

The display title of the preset

Examples:
`#$ TITLE: Example Preset`
`#$ TITLE: My Preset`

#### `AUTHOR`

The author of the preset

`#$ AUTHOR: Oats87`
`#$ AUTHOR: FDRC`

#### `FIRMWARE_VERSION`

The firmware version(s) that the preset is compatible with. Notably, this uses the `4.x` firmware version, rather than the Rotorflight `2.x` versioning.

##### Examples:
`#$ FIRMWARE_VERSION: 4.5.0` - Only compatible with 4.5.0
`#$ FIRMWARE_VERSION: 4.5` - Compatible with 4.5.x
`#$ FIRMWARE_VERSION: 4` - Compatible with 4.x

#### `CATEGORY`

The category of the preset.

Allowed Values:
|Value|Description|
|-----|-----------|
|`SETUP`|Presets for setup: things like receiver, mixer, etc.|
|`FILTERS`|Presets that set filters.|
|`PROFILE`|Presets that set profile settings.|
|`RATEPROFILE`|Presets that set rateprofile settings.|
|`REMAPPING`|Presets that perform board remapping for things like assigning ports to servo 5|
|`BNF`|Presets that perform a bind-and-fly setup for models, and should define the required FC/ESC/servos to use.|
|`OTHER`|Presets that don't fit into any of the above categories.|

#####Example:
`#$ CATEGORY: RATEPROFILE`

#### `STATUS`

Status of the preset.

##### Allowed Values: 
|Value|Description|
|-----|-----------|
|`COMMUNITY`|The standard status of a preset should be community, if it can be used.|
|`EXPERIMENTAL`|Experimental presets are presets that should be used with caution, for a variety of reasons.|
|`OFFICIAL`|Reserved for presets that are either published by the Rotorflight team or manufacturers that maintain their own presets.|

##### Example
`#$ STATUS: COMMUNITY`

#### `BOARD_NAME`

This tag defines the boards that the preset is compatible with. You can define multiple instances of this tag, if your preset is compatible with multiple boards.

##### Example
`#$ BOARD_NAME: FLYDRAGONF722_V2`

#### `DESCRIPTION` + `PARSER`

The description of the preset

##### Examples
```

```

#### `INCLUDE`

The filepath of another preset to include inline

#### `KEYWORDS`

Space-delimited list of keywords that will match when the preset is searched for.

#### `HIDDEN`
Hide the preset

#### `DISCUSSION`
A link to the discussion about the preset

#### `WARNING`
A warning that can be included and displayed with a preset.

#### `INCLUDE_WARNING`

#### `INCLUDE_DISCLAIMER`
A disclaimer that can be included and displayed with a preset.

#### `PRIORITY`
The priority of the preset when displayed in a list.

#### `FORCE_OPTIONS_REVIEW`
Force an options review before allowing the preset to be selected.

#### `OPTION_GROUP BEGIN`/`OPTION_GROUP END`/`OPTION BEGIN`/`OPTION END`

The options tags allow for the definition of selectable options, which changes the rendered preset. There are a few tags, namely `OPTION_GROUP` and `OPTION` that are required to create a functional option group.

For every option you want to provide, you must define the `OPTION_GROUP` tags twice, to indicate the start and end of the option group. These are marked with `#$ OPTION_GROUP BEGIN: <title>` and `#$ OPTION_GROUP END` accordingly. The options engine allows for options that are exclusive, if you want to restrict the user to only selecting one option at a time. If exclusivity is required, then the `(EXCLUSIVE)` tag should be added before the title of the option group, like `#$ OPTION_GROUP BEGIN: (EXCLUSIVE) Exclusive Option Title`.

Within the `OPTION_GROUP`, you can define your specific options, using `#$ OPTION BEGIN (CHECKED|UNCHECKED): <Option Value>` and `#$ OPTION END`. If you are using exclusive options, you can only have one `CHECKED` option. All other options should be marked as `UNCHECKED`. You can also have all options be `UNCHECKED` if not exclusive, and can use the `FORCE_OPTIONS_REVIEW` tag to ensure a user is required to view the options available.

An example of an option group is provided below:

```
#$ OPTION_GROUP BEGIN: (EXCLUSIVE) Exclusive Option Group
    #$ OPTION BEGIN (CHECKED): Option 1
    resource FREQ 1 A01
    #$ OPTION END
    #$ OPTION BEGIN (UNCHECKED): Option 2
    resource FREQ 1 A15
    #$ OPTION END
#$ OPTION_GROUP END
```

### Folder Structure

The folder structure for presets is as such.

1. All presets are contained in the `presets` folder. 
2. Presets should be nested under a folder in `presets` that is the author. For example, if `Oats87` wrote a preset, the presets for `Oats87` should be in `presets/Oats87`.
3. An author can come up with their own structure, but the general recommendation is to categorize your presets with a structure like: `presets/<author>/<category>/<firmware_version>/(board_name)`. If you don't have board restrictions, then it's not necessary to declare a board name.