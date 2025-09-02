# Discord.js Components v2 – A Deep Dive for Developers

Welcome to the ultimate guide for Discord.js Components v2.
Components v2 is not just a minor update—it’s a complete shift in how Discord messages can be structured, allowing you to build rich, interactive UIs inside Discord messages.

# Why Components v2 Matters

Traditionally, Discord messages were mostly plain text with occasional embeds. Components v2 turns messages into structured UI layouts, similar to mini web pages inside Discord:

Containers hold content blocks.

Sections group text and accessories.

Spacers & separators improve readability.

Media galleries allow multiple images in a single message.

Action rows provide interactivity.

Think of Components v2 as designing a Discord-native UI, where layout and flow are just as important as content.

Core Logic of Components v2

The heart of V2 is the container system:

Container – Base canvas (similar to <div> in HTML).

Inside it, you can add:

Sections

Text blocks

Media galleries

File attachments

Action rows

⚠️ Once MessageFlags.IsComponentsV2 is enabled, old features like content, embeds, and stickers are disabled. This encourages clean, organized layouts.

# Building Blocks Explained
1️⃣ ContainerBuilder – The Root

Every V2 message starts here.

const { ContainerBuilder } = require('discord.js'); // import necessary classes

const OweenContainer = new ContainerBuilder()
  .setAccentColor(0x5865F2) // Thin colored bar on the left
  .setSpoiler(false);        // Hide content if needed


💡 Logic: Accent color adds branding, spoilers control content visibility.

# 2️⃣ TextDisplayBuilder – Rich Text Blocks

Replaces embed titles/descriptions with independent rich text blocks.

const { TextDisplayBuilder } = require('discord.js');

const header = new TextDisplayBuilder()
  .setContent(`# Oween Panel Heading\n**Bold Italics**\n<@123456> works fine!`);


💡 Logic: Each text block is independent and supports large content, while respecting Discord’s message limit.

# 3️⃣ SectionBuilder – Grouping Information

Sections act like cards for grouping 1–3 text blocks plus accessories.

const { SectionBuilder, ButtonBuilder, ButtonStyle } = require('discord.js');

const mainSection = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent("Oween Main Info"),
    new TextDisplayBuilder().setContent("Oween Secondary Detail")
  )
  .setButtonAccessory(
    new ButtonBuilder()
      .setCustomId('oween_action')
      .setLabel('Take Action')
      .setStyle(ButtonStyle.Primary)
  );


💡 Logic: Sections create left-to-right flow: text left, accessory (button/image) right.

# 4️⃣ SeparatorBuilder – Visual Breathing Room
const { SeparatorBuilder, SeparatorSpacingSize } = require('discord.js');

const spacer = new SeparatorBuilder()
  .setSpacing(SeparatorSpacingSize.Large)
  .setDivider(true); // horizontal line


💡 Logic: Proper spacing improves readability. Don’t cram content.

# 5️⃣ MediaGalleryBuilder – Multiple Visuals
const { MediaGalleryBuilder, MediaGalleryItemBuilder } = require('discord.js');

const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder()
      .setURL('https://example.com/oween1.png')
      .setDescription('Oween showcase image 1'),
    new MediaGalleryItemBuilder()
      .setURL('https://example.com/oween2.jpg')
      .setDescription('Oween showcase image 2')
  );


💡 Logic: No need for multiple embeds; galleries allow multiple images in one message.

# 6️⃣ FileBuilder – Inline Attachments
const { FileBuilder } = require('discord.js');

const fileAttachment = new FileBuilder()
  .setURL('attachment://oween_doc.pdf')
  .setSpoiler(false);


💡 Logic: Files appear inline with the content, integrated rather than below the message.

# 7️⃣ ActionRowBuilder – Interactivity
const { ActionRowBuilder, SelectMenuBuilder } = require('discord.js');

const actions = new ActionRowBuilder()
  .addComponents(
    new ButtonBuilder()
      .setCustomId('oween_confirm')
      .setLabel('Confirm')
      .setStyle(ButtonStyle.Success),
    new SelectMenuBuilder()
      .setCustomId('oween_menu')
      .setPlaceholder('Select an option')
      .addOptions([
        { label: 'Option 1', value: 'opt1' },
        { label: 'Option 2', value: 'opt2' }
      ])
  );


# 💡 Logic: Action rows provide intuitive user decision points.

Layout Strategies
Vertical Flow (Simple)
OweenContainer
  .addTextDisplayComponents(header)
  .addSeparatorComponents(spacer)
  .addSectionComponents(mainSection)
  .addMediaGalleryComponents(gallery)
  .addActionRowComponents(actions);

Sectioned Content (Organized)
OweenContainer
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(title, description)
      .setThumbnailAccessory(preview)
  )
  .addSeparatorComponents(divider)
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(details)
      .setButtonAccessory(actionButton)
  );

Practical Examples
Oween Dashboard
const OweenDashboard = new ContainerBuilder()
  .setAccentColor(0x2F3136)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent("# Oween Server Dashboard")
  )
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent("Oween Online: 1,234"),
        new TextDisplayBuilder().setContent("Messages Today: 5,678")
      )
      .setButtonAccessory(
        new ButtonBuilder()
          .setCustomId('oween_refresh')
          .setLabel('Refresh')
          .setStyle(ButtonStyle.Secondary)
      )
  );

Oween Product Showcase
const OweenProduct = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent("## Oween Premium Bot\n**$19.99/month**")
  )
  .addMediaGalleryComponents(gallery)
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(
      new ButtonBuilder()
        .setCustomId('oween_buy')
        .setLabel('Buy Now')
        .setStyle(ButtonStyle.Success),
      new ButtonBuilder()
        .setCustomId('oween_more')
        .setLabel('Learn More')
        .setStyle(ButtonStyle.Primary)
    )
  );

Best Practices

Hierarchy First → Plan: header → sections → media → actions.

Less is More → Avoid too many buttons or text blocks.

Respect Limits → Max 40 components per message.

Consistent Spacing → Use separators for breathing room.

Mobile Awareness → Always test layouts on phones.

Closing Thoughts

Components v2 transforms Discord messages into flexible, interactive UI frameworks. Focus on:

Layout

Flow

Clarity

Start small: one container, one section, one button. Gradually add galleries, spacers, and action rows. With practice, you can create dashboards, product showcases, and mini-apps inside Discord.
