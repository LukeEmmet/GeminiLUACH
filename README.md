# Lightweight Unicode Author Client Hinting for Gemini (LUACH) - a proposal
This document outlines a draft proposal for lightweight hinting of author intent in gemini *text/gmi* files, in particular as textual decoration of links made.

### Status

The status of this document is draft - comments and suggestions are welcomed. 

### Author

Luke Emmet: luke [dot] emmet [at] gmail [dot] com

### History

* 0.1a, 4-Jun-2020, initial draft for discussion

## Background

Gemini specifies an exchange protocol and very lightweight text structuring, similar to a very small subset of markdown. This text format is generally expected to be UTF-8 for the overwhelming majority of content.

The general intention with Gemini is to keep the format very simple to be read by a range of users:

* Users and authors reading and writing the file as plain UTF-8 text
* Command line clients
* Graphical clients
* Search engines

The GMI text format is line oriented and includes the following key features only

* text
* links (prefixed =>)
* preformatted text (blocked out like markdown with ```)
* (optional) headings 1-3 (prefixed # to ###)
* (optional) bulleted list, single level (prefixed *)

### Gemini link format

Gemini/text specifies a simple yet effective mechanism to link to other pages. Often these will be other gemini pages, although other urls are permitted.

```
=>([WHITESPACE])URL([WHITESPACE]DISPLAYTEXT)
```

The DISPLAYTEXT in gemini is plain text (UTF-8) and under the control of the author or application generating the content.

## LUACH proposal

This proposal specifies a number of lightweight text conventions that authors **may** use to signal their intent for the linked content. It defines textual elements that may be added to the gemini link DISPLAYTEXT (see Gemini link format) by the author to hint to readers and clients the purpose of the link.

It is imperative to note that there is **no obligation** for any client to implement these LUACH signifiers, and even if authors put them into their content, they should write the content as if it is a simple link that can equally be used by a simple Gemini client as well as any that adopt the conventions below.

The intention is to provide a mechanism for readers to understand the role of the linked content, and optionally for certain client software to present the linked content in a helpful way.

### Design principles

* text based
* optional for authors, clients and readers
* provide useful and common hinting for human readers and software clients alike
* progressive enhancement
* consolidate an existing practice (for example of putting an envelope within the display text in an email address)
* no obligation as to implementation

### Supported protocols

Only content served over the gemini:// protocol may be further interpreted as described below. All other links **MUST** be shown as a normal simple link

## LUACH element signalling

The convention to be adopted is as follows (to be discussed)

### Embedded glyph

LUACH glyph as first or last word in DISPLAYTEXT may be interpreted as LUACH element. For example: 

*  "📦 A code snippet in python" 
* "Meine Startseite 🏠"

This is the preferred form and is not language specific - and can be used in any language

### LUACH text within square brackets 

LUACH text as the as first or last element, separated by whitespace may be interpreted as LUACH element (case insensitive) - For example: 

* "[Responds to] this discussion by another writer"
* "[Home]"

This form is Anglo-centric and thus limited to the community of readers and writers.

*Do we want to support this form?*

### Anywhere within the display text

Is this really needed? TBD. For example

* "Here is my [home] page"

*Possibly can leave this to a future iteration?*

## LUACH 1.0 set

The following LUACH elements are proposed as an initial set of hints. They can be adopted individually or as a set.

The behaviour when more than one LUACH element is found in a link DISPLAYTEXT is unspecified and left to the client.

Each element specifies its normal unicode glyph form or the anglo-centric textual form

### "Home" (U+1F3E0) 🏠

Intended meaning: *This is a link to my preferred "Home" URL.* 

Possible implementations might include:

* Present the home URL in a more prominent way on the page, perhaps in combination with any preferred linked logo.
* Integrate the link into the client generic UI navigational menus when navigating this site

### "Navigation" or "Menu" (U+1F9ED) 🧭

Intended meaning: *The linked page contains some useful common navigational links to help you navigate my site*

Clients who interpret this glyph **should** display those links in the linked content in a helpful way. Text and headings in the linked content **should** be ignored.

The only level of depth permitted is 1. That is to say no deeper LUACH elements in the included content are to be further interpreted.

**Only** links to content from the current domain **MAY** be interpreted. All other links URIs **MUST NOT** be interpreted, rather shown as simple links

Possible implementations might include:

* A popup menu that appears when the user activates the link
* An included table of common navigational links

Supported mime types

* text/gemini only, other mime types are left as simple links and must not be interpreted


### "May include" 📦 U+1F4E6

Intended meaning: *This linked content **may** optionally be shown in context here if desired.* 

The only level of depth permitted is **1**. That is to say no deeper LUACH elements in the included content are to be further interpreted.

Only links within the current domain **MAY** be interpreted and included. This is to prevent unexpected inclusion of third party content not under the control of the author.  Links to other domains **MUST NOT** be interpreted, and a simple link must be shown

If the linked content is text/gemini, no further "may include" links in the linked content may be interpreted.  This is to keep the simplicity down and prevent any expectation by authors or readers of complex interpretation or multiple layers of inclusion

Included content **MUST** be clearly signalled to the end user. For example using different colour or a boundary box around the included item.

Authors **MUST NOT** require, anticipate or otherwise expect the content will be included, users may choose to navigate the link themselves, to use a client that includes the content only when activated by the user, or generally includes such linked content. The behaviour is client and user determined.

The only supported mime types are: 

* text/gemini - the content may be flowed into the current page, suitably marked as included content
* text/plain - the content may be shown within a code fence, suitably marked as included content
* other mime types must not interpreted and are left as normal links (see below for separate proposal for images)

The exact mechanism software clients may offer to users is unspecified. The following are examples of possible interpretations:

* no action, but a visual decoration or further colour hinting may be shown
* convert the link to an active button that shows a popup form or includes the content when pressed by the user
* automatic inclusion of the content within a coloured boundary, configured on a domain basis, or globally

### "Has comments" 💬 U+1F4AC

Intended meaning: *The linked URL provides further details of comments on the current page.*

The intention is to link to a summary of comments and responses to this page. How the list of comments is assembled is optional, however automated crawlers can bots may assemble some of this picture by looking for "Comments on" elements.

### "Responds to" 💭 U+1F4AD

Intended meaning: *The current page provides commentary or response to the linked URL.*

Automated crawlers and bots can use the existence of such an element to build a database of commentary between pages.

### "Logo" (U+1F33C) 🌼 

Intended meaning: *This is my preferred logo that may be shown on this page.* 

The size and location of the display of the logo is determined by the client.

* **Scope of reference: Only** images from the current domain may be actively interpreted - others are left as simple links.

* Mime type: image/png only

### "May include image" (U+1F304) 🌄

Intended meaning: *You may display this linked image at or near the current location, if desired.* 

The size and location is determined by the client.

* **Scope of inclusion: Only** images from the current domain may be interpreted - others are left as simple links.

* Mime type: image/png or image/jpeg only - other mime types are left as simple links

*Possibly merge this with "May include"?*





