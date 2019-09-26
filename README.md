# SimpleTranslateTool
SimpleTranslateTool aims to provide a simple alternative way of managing multiple language translations in a .NET application.

The tool is currently focused on providing an alternative method for translating Windows Forms (WinForms) applications.  

.NET has built in capabilities for language translation. We intentionally take a different approach in order to provide an alternative solution for cases where the standard capabilities are not appropraite. For example, in a WinForms application the developer has to set the `Localizable` property to true on every form manually. If if this step is automated, the actually process of extract values for translation into a `resx` resource file is a blackbox process that occurs during the project build that can not be customised or called externally.

Main features of SimpleTranslateTool:
- Dynamically translate the application at run time, allowing users to switch among available languages in real time.
- Work with CSV and tabular formats in lieu of XML

Note: SimpleTranslateTool makes use of a single `resx` resource file that is built into the project. Alternatives have been considered (such as using SQLite to lookup the required translations) but these would add additional complexity and dependencies.

To use SimpleTranslateTool you must either, inherit from the SimpleTranslateTool base form, or include to SimpleTranslateTool on Load of each form.

## The translation table

We use a single resource file to provide a table of translations

| name                            | value                          | comment  |
|---------------------------------|--------------------------------|----------|
| en__form1__control1__property1  | `\|<<`  `>>\|`                 | E        |
| en__form1__control1__property2  | {"Item1", "Item2"}             |          |
| en__form2__control5__property1  | en__form1__control1__property2 | L        |
| fr__form1__control1__property2  | {"Article1", "Article2"}       |          |
| fr__form2__control5__property1  | fr__form1__control1__property2 | L        |

When sorted alphabetically, translations are grouped by language (e.g. `en`), form name, control name and then property name.

Note: Currently the default language is assumed to be English. The default language strings are hardcoded into the application at design
time and can be found in `*.Designer.vb` files. We also include every English string in the translation table for two reasons:

a) This allows us to detect when the text in the Designer files has changed, this allows us to mark translations in other languages as needing `Update`. The translated string cannot really be cleared or set to the default English, therefore the old translation will remain. The software shouldn't be released whilst updates are required.

b) Storing the English version allows to to change back to English very easily at runtime if the user had a different display language selected.

Flag currently being considered for the `comment` column:

| Flag | Description |
|------|-------------|
| E    | Excluded    |
| U    | Update      |
| N    | New         |
| L    | Link        |

When the `Excluded` flag is used, the default language version is used.

Entries marked `Update` and `New` have not yet been approved. Entries marked `Link` use the translation from the linked entry instead of creating a duplicate translation that needs to be maintained.
