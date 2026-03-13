---
source: https://www.swift.org/documentation/docc/adding-structure-to-your-documentation-pages
crawled: 2025-11-15T21:37:53Z
---

# Adding Structure to Your Documentation Pages

Article# Adding Structure to Your Documentation Pages

Make symbols easier to find by arranging them into groups and collections.## [Overview](/documentation/docc/adding-structure-to-your-documentation-pages#Overview)

By default, when DocC generates documentation for a project, it creates a top-level page that lists all public symbols, and groups them by their kind. You can then provide additional context to explain how your framework works and how different symbols relate to each other. For a more tailored learning experience, use one or more of the following approaches:

- Customize the main landing page for your documentation catalog to introduce your framework and organize its top-level symbols.

- Add symbol-specific extension files that organize nested symbols, such as methods and properties.

- Use collections to group multiple symbols and introduce hierarchy to the navigation of your documentation pages.

For more information, see [Writing Symbol Documentation in Your Source Files](/documentation/docc/writing-symbol-documentation-in-your-source-files).

### [Customize Your Documentation’s Landing Page](/documentation/docc/adding-structure-to-your-documentation-pages#Customize-Your-Documentations-Landing-Page)

A landing page provides an overview of your framework, introduces important terms, and organizes the resources within your documentation catalog — the files that enrich your source documentation comments. The landing page is an opportunity for you to ease the reader’s learning path, discuss key features of your technology, and offer motivation for the reader to return to when they need it.

For projects that don’t include a documentation catalog, DocC generates a basic landing page that provides an entry point to your framework’s symbol documentation. However, by adding a documentation catalog, you can include a custom landing page that provides a rich and engaging experience for adopters of your framework.

If you need to manually add a landing page to your documentation catalog, use your text editor to create a file to match the name of the framework. For example, for the `SlothCreator` framework, the filename is `SlothCreator.md`.

The first line of content in a landing page is an H1 heading containing the framework’s product module name, which you precede with a single hash (`#`) and encapsulate in a set of double backticks (``).



```
# ``SlothCreator``

```

Important

The name you use must match the compiled framework’s product name.

Follow the page title with a blank line to create a new paragraph. Then add a single sentence or sentence fragment, which DocC uses as the page’s abstract or summary.



```
# ``SlothCreator``


Catalog sloths you find in nature and create new adorable virtual sloths.

```

After the summary, add another blank line and then one or more paragraphs that introduce your framework to form the Overview section of the landing page. Try to keep the Overview brief — typically less than a screen’s worth of content. Avoid detailing every feature in your framework. Instead, provide content that helps the reader understand what problems the framework solves.

Write your Overview using *documentation markup*, a lightweight markup language that allows you to include images, lists, and links to symbols and other content. For more information, see [Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content).

In addition to presenting rich content, a custom landing page organizes the top-level symbols and other content in your documentation hierarchy.

### [Arrange Top-Level Symbols Using Topic Groups](/documentation/docc/adding-structure-to-your-documentation-pages#Arrange-Top-Level-Symbols-Using-Topic-Groups)

By default, DocC arranges the symbols in your framework according to their kind. For example, the compiler generates topic groups for classes, structures, protocols, and so forth. You then add information to explain the relationships between those symbols.

To help readers more easily navigate your framework, arrange symbols into groups with meaningful names. Place important symbols higher on the page, and nest supporting symbols inside other symbols. Use group names that are unique, mutually exclusive, and clear. Experiment with different arrangements to find what works best for you.

To override the default organization and manually arrange the top-level symbols in your framework, add a *Topics* section to your framework’s landing page. Below any content already in the Markdown file, add a double hash (`##`), a space, and the `Topics` keyword.



```
## Topics

```

After the Topics header, create a named section for each group using a triple hash (`###`), and add one or more top-level symbols to each section. Precede each symbol with a dash (`-`) and encapsulate it in a pair of double backticks (``) .



```
## Topics


### Creating Sloths


- ``SlothGenerator``
- ``NameGenerator``
- ``Habitat``


### Caring for Sloths


- ``Activity``
- ``CareSchedule``
- ``FoodGenerator``

```

Tip

 If you feel that a top-level topic group contains too many links, it could be an indication that that you can further organize the links into subtopics. For more information, see the [Incorporate Hierarchy in Your Navigation](/documentation/docc/adding-structure-to-your-documentation-pages#Incorporate-Hierarchy-in-Your-Navigation) section below.

DocC uses the double backtick format to create symbol links, and to add the symbol’s type information and summary. For more information, see [Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content).

When you rebuild your documentation, the documentation viewer reflects these organizational changes in the navigation pane and on the landing page, as the image above shows.

### [Arrange Nested Symbols in Extension Files](/documentation/docc/adding-structure-to-your-documentation-pages#Arrange-Nested-Symbols-in-Extension-Files)

Not all symbols appear on the top-level landing page. For example, classes and structures define methods and properties, and in some cases, nested classes or structures introduce additional levels of hierarchy.

As with the top-level landing page, DocC generates default topic groups for nested symbols according to their type. Use extension files to override this default organization and provide a more appropriate structure for your symbols.

To add an extension file for a specific symbol, use a text editor to create an `.md` file within the documentation catalog. DocC ignores file names of documentation extensions; you can choose any file name you would like as long as it uses an `.md` extension. (DocC determines the URL path of source documentation from the symbol’s name, type and parent type among other things.) Then open the file and add a level 1 header on the first line. Finally, set the level 1 header’s title to be a link to the corresponding symbol.

Here’s an example of a level 1 header set to be a symbol link:



```
# ``SlothCreator/Sloth``

```

Note

If you’re working in Xcode, you can select the documentation catalog in the project navigator and run the “File -> New -> File From Template…” menu command. Then click “Extension File” under the Documentation section. In the new markdown file, replace the “Symbol” placeholder with a link to the corresponding symbol, and rename the file accordingly.

Important

 If DocC can’t resolve the symbol link in the level 1 header of the extension file, it will raise a warning about the link and skip the rest of the content of that extension file.

DocC resolves symbol links in extension file headers relative to the module. To create an extension file for a nested type or member symbol, start the symbol link with the name of the top-level symbol that contains the nested type or member symbol. You can optionally prefix the symbol link with the name of the module, but DocC does not require this prefix.

The Extension File template includes a `Topics` section with a single named group, ready for you to fill out. Alternatively, if your documentation catalog already contains an extension file for a specific symbol, add a `Topics` section to it by following the steps in the previous section.

As with the landing page, create named sections for each topic group using a triple hash (`###`), and add the necessary symbols to each section using the double backtick (``) syntax.



```
# ``SlothCreator/Sloth``


## Topics


### Creating a Sloth


- ``init(name:color:power:)``
- ``SlothGenerator``


### Activities


- ``eat(_:quantity:)``
- ``sleep(in:for:)``


### Schedule


- ``schedule``

```

Tip

Use a symbol’s full path to include it from elsewhere in the documentation hierarchy.

After you arrange nested symbols in an extension file, use DocC to compile your changes and review them in your browser.

Note

 If you organize a member symbol into a topic group outside of the type that defines the member, DocC will still include the member symbol in the default topic group. This ensures that the reader can always find the member symbol somewhere in the sub-hierarchy of the containing type.

### [Incorporate Hierarchy in Your Navigation](/documentation/docc/adding-structure-to-your-documentation-pages#Incorporate-Hierarchy-in-Your-Navigation)

Much like you organize symbols on a landing page or in an extension file, you can create collections of symbols to add hierarchy to your documentation or to group symbols that have relationships other than those you define in your framework’s type hierarchy. For more information, see
[Adding Supplemental Content to a Documentation Catalog](/documentation/docc/adding-supplemental-content-to-a-documentation-catalog).

Collections are almost identical to articles, except for two things:

- A collection contains a Topics section, which instructs DocC to treat what would otherwise be an article as a collection.

- A collection rarely has enough descriptive content to warrant sections. Include a summary and an Overview section. If the Overview becomes long, consider turning it into an article and linking to it from one of the collection’s topic groups.

To link to a collection, use the less-than symbol (`<`), the `doc` keyword, a colon (`:`), the name of the collection, and the greater-than symbol (`>`). Don’t include the collection’s file extension in the name.



```
### Creating Sloths


- <doc:SlothGenerators>

```

DocC uses the collection’s filename for its URL, and its page title as the link text.

Collections are an important tool for bringing order to your documentation, but they can also confuse a reader if you create too many levels of hierarchy. Avoid using a collection when a topic group at a higher level can achieve the same result.

## [See Also](/documentation/docc/adding-structure-to-your-documentation-pages#see-also)

### [Structure and Formatting](/documentation/docc/adding-structure-to-your-documentation-pages#Structure-and-Formatting)

[Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content)Enhance your content’s presentation with special formatting and styling for text and lists.[Adding Tables of Data](/documentation/docc/adding-tables-of-data)Arrange information into rows and columns.[Other Formatting Options](/documentation/docc/other-formatting-options)Improve the presentation and structure of your content by incorporating special page elements.[Adding Images to Your Content](/documentation/docc/adding-images)Elevate your content’s visual appeal by adding images.[Customizing the Appearance of Your Documentation Pages](/documentation/docc/customizing-the-appearance-of-your-documentation-pages)Customize the look and feel of your documentation webpages.
- [ Adding Structure to Your Documentation Pages ](/documentation/docc/adding-structure-to-your-documentation-pages#app-top)

- [ Overview ](/documentation/docc/adding-structure-to-your-documentation-pages#Overview)

- [ Customize Your Documentation’s Landing Page ](/documentation/docc/adding-structure-to-your-documentation-pages#Customize-Your-Documentations-Landing-Page)

- [ Arrange Top-Level Symbols Using Topic Groups ](/documentation/docc/adding-structure-to-your-documentation-pages#Arrange-Top-Level-Symbols-Using-Topic-Groups)

- [ Arrange Nested Symbols in Extension Files ](/documentation/docc/adding-structure-to-your-documentation-pages#Arrange-Nested-Symbols-in-Extension-Files)

- [ Incorporate Hierarchy in Your Navigation ](/documentation/docc/adding-structure-to-your-documentation-pages#Incorporate-Hierarchy-in-Your-Navigation)

- [ See Also ](/documentation/docc/adding-structure-to-your-documentation-pages#see-also)