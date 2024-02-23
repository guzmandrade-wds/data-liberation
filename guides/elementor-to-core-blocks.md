# Elementor to WordPress Core blocks

🚧 **This Guide is a Work in Progress! We Need Your Help!** 🚧

We are in the process of creating a comprehensive guide to help users migrate from Elementor to WordPress Core blocks. Your insights and contributions are invaluable to make this guide more complete and helpful for the community.

## How You Can Contribute

-   **Review and Edit:** If you have experience with Elementor to Core block migration, review the existing content, and make edits for accuracy and clarity.

-   **Add Missing Steps:** If you notice any steps missing or have additional tips, please contribute by adding them to the guide.

-   **Share Your Experience:** Share your personal experience or best practices that can benefit others going through the migration process.

## Elementor to Core Blocks

Elementor maintains two copies of content edited with it, an HTML representation of the content without the page builder features stored in `post_content` and a JSON version stored in the `postmeta` table with much more information.

If the user decides to stop using Elementor by disabling or removing the WordPress plugin, or by using the option `Back to WordPress Editor`, the simpler HTML representation is what the users are left with.

On the one hand, Elementor provides the option for users to continue using their content even without Elementor, which is good, but the structure is lost in the process. Without Elementor, all the content of a page is stored as a Classic Editor block. Data such as sections, containers, rows, columns, styles, embedded content, etc., gets lost in this process.

## Recovering structural data

It is possible to read and parse the Elementor information using the post metadata, and there are several Elementor widgets with comparable functionalities than Core Blocks. The following are the blocks we could use to replace the Elementor Widgets, if we consider only the basic Elementor version.

| Elementor Widget | Core Block or Blocks                |
| ---------------- | ----------------------------------- |
| Accordion        | Details Blocks                      |
| Alert            | Heading and Paragraph Blocks        |
| Button           | Button Block inside a Buttons Block |
| Divider          | Separator                           |
| Heading          | Heading                             |
| HTML             | HTML Block                          |
| Icon             | Currently no Core Block equivalent  |
