# Elementor to WordPress Core blocks

🚧 **This Guide is a Work in Progress! We Need Your Help!** 🚧

We are in the process of creating a comprehensive guide to help users migrate from Elementor to WordPress Core blocks. Your insights and contributions are invaluable to make this guide more complete and helpful for the community.

## How You Can Contribute

- **Review and Edit:** If you have experience with Elementor to Core block migration, review the existing content, and make edits for accuracy and clarity.

- **Add Missing Steps:** If you notice any steps missing or have additional tips, please contribute by adding them to the guide.

- **Share Your Experience:** Share your personal experience or best practices that can benefit others going through the migration process.

## Elementor to Core Blocks

Elementor maintains two copies of the post content, an HTML representation without the page builder features stored in `post_content`, and a JSON version stored in the `postmeta` table (usually with `_elementor_data` as `meta_key`) with additional data.

If a page or post was edited with Elementor and later the user decides to stop using it by deactivating or uninstalling the WordPress plugin, or by using the option `Back to WordPress Editor` (available on the edit page as a way to use the WordPress Editor instead of Elementor), the simpler HTML representation is what the post content becomes.

Without Elementor, the post content is stored as a Classic Editor block (since it's raw HTML). Data such as sections, containers, rows, columns, styles, embedded content, etc., gets lost is the user decides to stop using the page builder.

## Recovering structural data

It is possible to read and parse the Elementor information using the postmeta data, and there are several Elementor widgets with comparable functionalities than Core Blocks. The following are the blocks we could use to replace the Elementor Widgets, if we consider only the basic Elementor version.

| Elementor Widget | Core Blocks                             |
| ---------------- | --------------------------------------- |
| Accordion        | Detail                                  |
| Alert            | Heading and Paragraph                   |
| Button           | Buttons and Button                      |
| Google Map       | Currently no Core Block equivalent      |
| Divider          | Separator                               |
| Heading          | Heading                                 |
| HTML             | HTML Block                              |
| Icon             | Currently no Core Block equivalent [^1] |
| Icon List        | Currently no Core Block equivalent [^1] |
| Icon Box         | Currently no Core Block equivalent [^1] |
| Image Carosel    | Currenly no Core Block equivalent       |
| Image Gallery    | Image Gallery Block                     |
| Image            | Image Block                             |
| Menu Anchor      | Classic Editor or HTML Block            |
| Spacer           | Spacer Block                            |
| Tabs             | Currenly no Core Block equivalent       |
| Testimonial      | Quote Block or Pullquote Blocks         |
| Text Editor      | Text Blocks                             |
| Toggle           | Detail                                  |
| Video            | Video Block                             |

Each Elementor Widget has its own structure and parameters, for example, the Widget Accordion stores the following data in `_elementor_data`:

```json
{
    "id": "<WIDGET_ID>",
    "elType": "widget",
    "settings": {
        "tabs": [
            {
                "tab_title": "A1",
                "tab_content": "<p>TEST 1</p>",
                "_id": "<TAB_ID>"
            },
            {
                "tab_title": "A2",
                "tab_content": "<p>TEST 2</p>",
                "_id": "<TAB_ID>"
            }
        ]
    },
    "elements": [],
    "widgetType": "accordion"
}
```

Which is possible to parse and use the array of `tabs` to create a group of Detail Blocks with that information.

[^1]: Elementor uses Font Awesome to render icons, so it is possible to replace the icon part of these widgets with Font Awesome icons. Therefore, one could add Font Awesome to the target system and replace Elementor Icons with an Core HTML Block and using the icons with `<i>` or `<span>` elements. More information: <https://docs.fontawesome.com/v5/web/reference-icons/>

### Classic WordPress Widgets

Elementor includes the feature of adding Classic WordPress widgets as Elementor widgets. These special widgets are stored in the post meta table as `widgetType` with value of `wp-widget-{widget}`. To add these WordPress widgets as Gutenberg blocks, it is possible to add support for the Block `core/legacy-widget`. This can be done in the `functions.php` of the target theme, as documented in the [WP.org docs](https://developer.wordpress.org/block-editor/how-to-guides/widgets/legacy-widget-block/#using-the-legacy-widget-block-in-other-block-editors-advanced).

### Sections, Columns, Containers

Elementor is [transitioning from Sections and Columns to Containers](https://elementor.com/help/transitioning-from-sections-to-containers/). With the migration from Elementor to Gutenberg Blocks in mind, this means the postmeta information in the database can be on either of those formats.

- Sections and Columns. The structure is similar to Group Blocks, Stack Blocks and Columns Blocks.
- Containers. They have FlexBox attributes, which can be migrated into Gutenberg Blocks as Stacks or Grids [^2]

[^2]: This is a recent addition to Gutenberg, for more information, please go to: <https://developer.wordpress.org/news/2024/02/06/adding-and-using-grid-support-in-wordpress-themes/>.

### Migrating Elementor Widgets from Add-ons

Elementor can be extended to create custom Widgets. Those custom Widgets store their parameters and attributes as part of the main `_elementor_data` postmeta value. To migrate those to Gutenberg Blocks, it is possible to examine that object and parse it to convert those Widgets to equivalent Gutenberg Blocks.

For example, the following is a representation of the data for the widget `premium-addon-button` (a Widget part of the WordPress plugin `Premium Addons for Elementor`):

```json
{
    "id": "<WIDGET_ID>",
    "elType": "widget",
    "settings": {
        "premium_button_text": "<BUTTON_TEXT>",
        "premium_button_link": {
            "url": "<BUTTON_URL>",
            "is_external": "",
            "nofollow": "",
            "custom_attributes": ""
        },
        "pa_condition_repeater": []
    },
    "elements": [],
    "widgetType": "premium-addon-button"
}
```

And to convert this into Blocks, we can use these parameters to create a new structure based on the Buttons and Button Blocks.
