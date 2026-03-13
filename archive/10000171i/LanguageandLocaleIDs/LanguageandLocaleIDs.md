---
title: "Appendix B: Language and Locale IDs"
book: "Internationalization and Localization Guide"
chapterId: "10000171i-CH15"
date: "2015-09-16"
description: "Explains how to create a user interface and write code that can be localized into multiple languages."
identifier: "//apple_ref/doc/uid/10000171i"
source: apple-archive
---

# Appendix B: Language and Locale IDs

> **Internationalization and Localization Guide**
> Last updated: 2015-09-16

[Next](../StringsdictFileFormat/StringsdictFileFormat.html)[Previous](../MaintaingYourOwnStringsFiles/MaintaingYourOwnStringsFiles.html)
        
        
        [](../index.html)
        
        # Language and Locale IDs
Language IDs identify a language, dialect, or script and are used to name language-specific resource folders stored in the app bundle. Locale IDs identify a set of regional conventions and are used in APIs—such as the `[NSLocale](https://developer.apple.com/documentation/foundation/nslocale)`, `[NSDateFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateFormatter/Description.html#//apple_ref/occ/cl/NSDateFormatter)`, `[NSNumberFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumberFormatter/Description.html#//apple_ref/occ/cl/NSNumberFormatter)`, and `[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)` classes—where region information is needed to format data. OS X and iOS use standard language ID and locale ID formats that consist of language and region designators. For example, using a language combined with a region designator, a language ID can distinguish between different languages and regional dialects.

## Language Designators
A *language designator* is a code that represents a language. Use the two-letter ISO 639-1 standard (preferred) or the three-letter ISO 639-2 standard. If an ISO 639-1 code is not available for a particular language, use the ISO 639-2 code instead. For example, there is no ISO 639-1 code for the Hawaiian language, so use the ISO 639-2 code. Table B-1 lists language designators for a subset of languages. 

**Table B-1**  Language designator examplesLanguage

ISO 639-1 Code

ISO 639-2 Code

English

`en`

`eng`

French

`fr`

`fre`

German

`de`

`ger`

Japanese

`ja`

`jpn`

Hawaiian

no designator

`haw`

For a complete list of ISO 639-1 and ISO 639-2 codes, see [ISO 639.2 Codes for the Representation of Names and Languages](http://www.loc.gov/standards/iso639-2/php/English_list.php). 

## Region Designators
A *region designator* is a code that represents a country. Use the ISO 3166-1 standard, a two-letter, capitalized code shown in Table B-2. 

**Table B-2**  Regional designator examplesRegion

ISO 3166-1 Code

United States

`US`

United Kingdom

`GB`

Australian

`AU`

France

`FR`

Canadian

`CA`

For a complete list of ISO 3166-1 codes, see [Online Browsing Platform (OBP) for ISO country codes](https://www.iso.org/obp/ui/#search). 

## Language IDs
A *language ID* identifies a language used in many regions, a dialect used in a specific region, or a script used in multiple regions. To specify a language used in many regions, use a language designator by itself. To specify a specific dialect, use a hyphen to combine a language designator with a region designator. To specify a script, combine a language designator with a script designator. For example, to specify common English, use the `en` language designator as the language ID. To specify the English language as it is used in the United Kingdom, use `en-GB` as the language ID. 

Table B-3 shows the supported language ID syntax and examples of common languages and dialects.

**Table B-3**  Language ID syntax and examplesLanguage ID syntax

Examples

Description

`[language designator]`

`en` for English

`fr` for French

`de` for German

Specifies a language only.

`[language designator]-[region designator]`

`en-AU` for English as used in Australia

`en-GB` for English as used in United Kingdom

`fr-FR` for French as used in France

`fr-CA` for French as used in Canada

`de-AT` for German as used in Austria

`de-CH` for German as used in Switzerland

Specifies a dialect of a language.

`[language designator]-[script designator]`

See Table B-4.

Specifies a script of a language.

For the script designator, use the [ISO 15924](http://www.unicode.org/iso15924/iso15924-codes.html) standard, four letters with the first letter uppercase and the last three lowercase, as shown in Table B-4.

**Table B-4**  Script language ID examplesScript language ID

Description

`az-Arab`

Azerbaijani in the Arabic script.

`az-Cyrl`

Azerbaijani in the Cyrillic script.

`az-Latn`

Azerbaijani in the Latin script.

`sr-Cyrl`

Serbian in the Cyrillic script.

`sr-Latn`

Serbian in the Latin script.

`uz-Cyrl`

Uzbek in the Cyrillic script.

`uz-Latn`

Uzbek in the Latin script.

`zh-Hans`

Chinese in the simplified script. 

`zh-Hant`

Chinese in the traditional script.

See the “ISO 639-3 and Macro Languages” section of [Understanding the New Language Tags](http://www.w3.org/International/articles/bcp47/) for more Chinese language ID examples. For the complete BCP 47 specification for language tags, go to [BCP 47: Tags for Identifying Languages](ftp://ftp.isi.edu/in-notes/bcp/bcp47.txt). However, iOS and OS X only support the language ID syntax consisting of a language designator and optional region or script designator. 

## Locale IDs
A *locale ID* identifies a specific region and its cultural conventions—such as the formatting of dates, times, and numbers. To specify a locale, use an underscore character to combine a language ID with a region designator, as shown in Table B-5. For example, the locale ID for English-language speakers in the United Kingdom is `en_GB`, while the locale for English-speaking residents of the United States is `en_US`. 

**Table B-5**  Locale ID syntax and examplesLocale ID syntax

Examples

Description

`[language designator]`

`en`

`fr`

An unspecified region where the language is used.

`[language designator]_[region designator]`

`en_GB`

`zh_HK`

The language used by and regional preference of the user.

`[language designator]-[script designator]`

`az-Arab`

`zh-Hans`

An unspecified region where the script is used.

`[language designator]-[script designator]_[region designator]`

`zh-Hans_HK`

The script used by and regional preference of the user.

 

Only use a script designator in a locale ID when there is ambiguity. For example, because Traditional Chinese is the default language in Hong Kong, use `zh_HK`, where `zh` is the code for Traditional Chinese and `HK` is the code for the Hong Kong region. For Simplified Chinese used in Hong Kong, use `zh-Hans_HK` as the locale ID, where `zh-Hans` is the code for the Simplified Chinese script. 

## Using Subtag Designators
If necessary, you can use a language or locale code that is not known to the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class or Core Foundation bundle functions. For example, you could create your own language designators for a language that is not yet listed in the ISO conventions or available as a language in Xcode.

If you choose to create a new designator, be sure to follow the rules found in sections 2.2.1 and 4.5 of [BCP 47: Tags for Identifying Languages](ftp://ftp.isi.edu/in-notes/bcp/bcp47.txt). Tags that do not follow these conventions are not guaranteed to work. When using subtags, ensure that the abbreviation stored by the user’s language settings matches the designator used by your `.lproj` directory exactly. 

        
            [Next](../StringsdictFileFormat/StringsdictFileFormat.html)[Previous](../MaintaingYourOwnStringsFiles/MaintaingYourOwnStringsFiles.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-09-16