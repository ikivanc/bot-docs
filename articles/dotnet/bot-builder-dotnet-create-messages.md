---
title: Create messages with the Bot Framework Connector service | Microsoft Docs
description: Learn how to create and format the different types of messages your bot can use to communicate.
author: kbrandl
ms.author: v-kibran
manager: rstand
ms.topic: article
ms.prod: bot-framework

ms.date: 03/13/2017
ms.reviewer: rstand
ROBOTS: Index, Follow
---

# Create messages

Your bot will send **message** [activities](~/dotnet/bot-builder-dotnet-activities.md) to communicate information to users, 
and likewise, will also receive **message** activities from users. 
Some messages may simply consist of plain text, while others may contain richer content such as 
[media attachments](~/dotnet/bot-builder-dotnet-add-media-attachments.md), [buttons, and cards](~/dotnet/bot-builder-dotnet-add-rich-card-attachments.md). 
This article describes some of the commonly-used message properties.

## Message text and format

To create a basic message that contains only plain text, simply specify the `Text` property (as contents of the message) 
and the `Locale` property (as the locale of the sender). 

[!code-csharp[Set message properties](~/includes/code/dotnet-create-messages.cs#setBasicProperties)]

The `TextFormat` property of a message can be used to specify the format of the text. 
`TextFormat` defaults to `markdown` and interprets text using Markdown formatting standards, 
but you can also specify `plain` to interpret text as plain text or `xml` to interpret text as XML markup.

If a channel is not capable of rendering the specified formatting, the message will be rendered using reasonable approximation. For example, Markdown **bold** text for a message sent via text messaging will be rendered as \*bold\*. Only channels that support fixed width formats and HTML can render standard table markdown.

These styles are supported when `TextFormat` is set to `markdown`:

| Style | Markdown | Example | 
| ---- | ---- | ---- | 
| bold | \*\*text\*\* | **text** |
| italic | \*text\* | *text* |
| header (1-5) | # H1 | #H1 |
| strikethrough | \~\~text\~\~ | ~~text~~ |
| horizontal rule | --- |  |
| unordered list | \* text |  <ul><li>text</li></ul> |
| preformatted text | \`text\` |  |
| blockquote | \> text | <blockquote>text</blockquote> |
| hyperlink | \[bing](http://www.bing.com) | [bing](http://www.bing.com) |
| image link| \[duck](http://aka.ms/Fo983c) | [duck](http://aka.ms/Fo983c) |

These styles are supported when `TextFormat` is set to "xml":
> [!CAUTION]
> The `xml` value of for `TextFormat` is supported by the Skype channel only. 

| Style | Markdown | Example | 
|----|----|----|----|
| bold | \<b\>text\</b\> | **text** | 
| italic | \<i\>text\</i\> | *text* |
| underline | \<u\>text\</u\> | <u>text</u> |
| strikethrough | \<s\>text\</s\> | <s>text</s> |
| hyperlink | \<a href="http://www.bing.com"\>bing\</a\> | <a href="http://www.bing.com">bing</a> |

## Message attachments

The `Attachments` property of a message activity can be used to send and receive simple media attachments 
(image, audio, video, file) and rich cards. 
For details, see [Add media attachments to messages](~/dotnet/bot-builder-dotnet-add-media-attachments.md) and 
[Add rich cards to messages](~/dotnet/bot-builder-dotnet-add-rich-card-attachments.md).

## Message entities

The `Entities` property of a message is an array of open-ended <a href="http://schema.org/" target="_blank">schema.org</a> 
objects which allows the exchange of common contextual metadata between the channel and bot.

### Mention entities

Many channels support the ability for a bot or user to "mention" someone within the context of a conversation. 

To mention a user in a message, populate the message's `Entities` property with a `Mention` object. 
The `Mention` object contains these properties: 

| Property | Description | 
|----|----|
| type | type of the entity ("mention") | 
| mentioned | `ChannelAccount` object that indicates which user was mentioned | 
| text | text within the `Activity.Text` property that represents the mention itself (may be empty or null) |

This code example shows how to add a `Mention` entity to the `Entities` collection.

[!code-csharp[set Mention](~/includes/code/dotnet-create-messages.cs#setMention)]

> [!TIP]
> When attempting to determine user intent, the  bot may want to ignore that portion
> of the message where it is mentioned. Call the `GetMentions` method and evaluate
> the `Mention` objects returned in the response.

### Place objects

<a href="https://schema.org/Place" target="_blank">Location-related information</a> can be conveyed 
within a message by populating the message's `Entities` property with either 
a `Place` object or a `GeoCoordinates` object. 

The `Place` object contains these properties:

| Property | Description | 
|----|----|
| Type | type of the entity ("Place") |
| Address | description or `PostalAddress` object (future) | 
| Geo | GeoCoordinates | 
| HasMap | URL to a map or `Map` object (future) |
| Name | name of the place |

The `GeoCoordinates` object contains these properties:

| Property | Description | 
|----|----|
| Type | type of the entity ("GeoCoordinates") |
| Name | name of the place |
| Longitude | longitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>) | 
| Longitude | latitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>) | 
| Elevation | elevation of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>) | 

This code example shows how to add a `Place` entity to the `Entities` collection:

[!code-csharp[set GeoCoordinates](~/includes/code/dotnet-create-messages.cs#setGeoCoord)]

### Consume entities

To consume entities, use either the `dynamic` keyword or strongly-typed classes.

This code example shows how to use the `dynamic` keyword to process an entity within the `Entities` property of a message:

[!code-csharp[examine entity using dynamic keyword](~/includes/code/dotnet-create-messages.cs#examineEntity1)]

This code example shows how to use a strongly-typed class to process an entity within the `Entities` property of a message:

[!code-csharp[examine entity using typed class](~/includes/code/dotnet-create-messages.cs#examineEntity2)]

## Message channel data

The `ChannelData` property of a message activity can be used to implement channel-specific functionality. 
For details, see [Implement channel-specific functionality](~/dotnet/bot-builder-dotnet-channeldata.md).

## Additional resources

- [Activity types](~/dotnet/bot-builder-dotnet-activities.md)
- [Send and receive activities](~/dotnet/bot-builder-dotnet-connector.md)
- [Add media attachments to messages](~/dotnet/bot-builder-dotnet-add-media-attachments.md)
- [Add rich cards to messages](~/dotnet/bot-builder-dotnet-add-rich-card-attachments.md)
- [Implement channel-specific functionality](~/dotnet/bot-builder-dotnet-channeldata.md)