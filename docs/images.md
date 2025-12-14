# Retrieving Quran Images: Indo-Pak Font and Word Frames

This guide explains how to retrieve images for verses and individual words in different font styles, including Indo-Pak font.

## Overview

The Quran.com API provides two types of images:
1. **Verse Images** - Complete verse rendered in different font styles
2. **Word Images/Frames** - Individual word images for word-by-word display

## Verse Images

### Getting Verse Images with Indo-Pak Font

Use the `/quran/verses/{script}` endpoint to retrieve verse images. For Indo-Pak font images, use the `v1_image` script parameter.

**Endpoint:**
```
GET /api/v4/quran/verses/v1_image
```

**Query Parameters:**
- `chapter_number` - Chapter number (1-114)
- `juz_number` - Juz number (1-30)
- `page_number` - Page number (1-604)
- `verse_key` - Specific verse key (e.g., "1:1")
- `hizb_number` - Hizb number
- `rub_el_hizb_number` - Rub el Hizb number

**Example Request:**
```bash
# Get all verses from Chapter 1 with image URLs
curl "https://api.quran.com/api/v4/quran/verses/v1_image?chapter_number=1"

# Get specific verse image
curl "https://api.quran.com/api/v4/quran/verses/v1_image?verse_key=1:1"

# Get all verses from a specific page
curl "https://api.quran.com/api/v4/quran/verses/v1_image?page_number=1"
```

**Example Response:**
```json
{
  "verses": [
    {
      "id": 1,
      "verse_key": "1:1",
      "image_url": "//cdn.quran.com/images/1_1.png",
      "image_width": 675
    }
  ]
}
```

### Getting Verse Text with Indo-Pak Script

To get the Indo-Pak text (not images), use the `/quran/verses/indopak` endpoint:

**Endpoint:**
```
GET /api/v4/quran/verses/indopak
```

**Example Request:**
```bash
# Get Indo-Pak text for Chapter 1
curl "https://api.quran.com/api/v4/quran/verses/indopak?chapter_number=1"
```

**Example Response:**
```json
{
  "verses": [
    {
      "id": 1,
      "verse_key": "1:1",
      "text_indopak": "بِسۡمِ اللهِ الرَّحۡمٰنِ الرَّحِيۡمِ"
    }
  ]
}
```

## Word Images/Frames

Word images (word frames) allow you to display individual words with their visual representation, useful for word-by-word highlighting and display.

### Getting Words with Image URLs

Use the `/verses` endpoints with the `word_fields` parameter to include word image URLs.

**Endpoint:**
```
GET /api/v4/verses/{filter}
```

Where `{filter}` can be:
- `by_chapter/{chapter_number}`
- `by_page/{page_number}`
- `by_juz/{juz_number}`
- `by_key/{verse_key}`
- etc.

**Query Parameters:**
- `word_fields` - Comma-separated list of word fields including `image_url` and `image_blob`
- `words` - Set to `true` to include words in the response

**Example Request:**
```bash
# Get verse with word images for Indo-Pak font
curl "https://api.quran.com/api/v4/verses/by_chapter/1?words=true&word_fields=text_indopak,image_url,image_blob,position,location"

# Get specific verse with word frames
curl "https://api.quran.com/api/v4/verses/by_key/1:1?words=true&word_fields=text_indopak,image_url,position"

# Get page with word images
curl "https://api.quran.com/api/v4/verses/by_page/1?words=true&word_fields=text_indopak,image_url,image_blob"
```

**Example Response:**
```json
{
  "verses": [
    {
      "id": 1,
      "verse_key": "1:1",
      "words": [
        {
          "id": 1,
          "position": 1,
          "text_indopak": "بِسۡمِ",
          "image_url": "//cdn.quran.com/words/1_1_1.png",
          "location": "1:1:1"
        },
        {
          "id": 2,
          "position": 2,
          "text_indopak": "اللهِ",
          "image_url": "//cdn.quran.com/words/1_1_2.png",
          "location": "1:1:2"
        }
      ]
    }
  ]
}
```

### Available Word Fields

The following fields are available for words when using the `word_fields` parameter:

**Text Fields:**
- `text_uthmani` - Uthmani script
- `text_indopak` - Indo-Pak script
- `text_imlaei` - Imlaei script
- `text_imlaei_simple` - Simple Imlaei script
- `text_uthmani_simple` - Simple Uthmani script
- `text_uthmani_tajweed` - Uthmani with tajweed marks
- `text_qpc_hafs` - QPC Hafs script

**Image Fields:**
- `image_url` - URL to the word image
- `image_blob` - Base64-encoded image data (if available)

**Position Fields:**
- `position` - Word position in the verse
- `location` - Word location (chapter:verse:word format)
- `page_number` - Page number (Madani Mushaf)
- `v1_page` - Page number (version 1)
- `v2_page` - Page number (version 2)
- `line_number` - Line number on the page

**Other Fields:**
- `verse_id` - Verse ID
- `chapter_id` - Chapter ID
- `verse_key` - Verse key (chapter:verse format)
- `audio_url` - Audio URL for the word
- `char_type_name` - Character type name

## Using with GraphQL

You can also retrieve word images using the GraphQL API:

**GraphQL Query:**
```graphql
query {
  verses(chapter: 1) {
    id
    verseKey
    words {
      id
      position
      textIndopak
      imageUrl
      imageBlob
      location
    }
  }
}
```

**Example GraphQL Request:**
```bash
curl -X POST https://api.quran.com/graphql \
  -H "Content-Type: application/json" \
  -d '{
    "query": "query { verses(chapter: 1) { id verseKey words { id position textIndopak imageUrl location } } }"
  }'
```

## Font Types and Scripts

The API supports multiple font types and scripts:

1. **Indo-Pak Fonts:**
   - `text_indopak` - Indo-Pak script with me_quran font
   - `text_indopak_nastaleeq` - Normal Indopak script (Alqalam Quran font)

2. **QPC Fonts:**
   - `text_qpc_hafs` - QPC Uthmani Hafs
   - `text_qpc_nastaleeq` - QPC Nastaleeq (compatible with indopak font)
   - `text_qpc_nastaleeq_hafs` - QPC Nastaleeq (compatible with QPC font)

3. **Other Fonts:**
   - `text_uthmani` - Standard Uthmani script
   - `text_uthmani_tajweed` - Uthmani with tajweed color coding
   - `text_uthmani_simple` - Simplified Uthmani script
   - `text_imlaei` - Modern spelling
   - `text_imlaei_simple` - Simplified modern spelling

## Image CDN

Word and verse images are typically served from a CDN. The `image_url` field contains relative URLs that should be prefixed with the appropriate CDN domain (e.g., `https://cdn.quran.com` or `https://static.quran.com`).

## Notes

1. **Image Availability:** Not all words may have image URLs populated in the database. The availability depends on the font type and data import status.

2. **Image Blob:** The `image_blob` field contains base64-encoded image data. This is useful for offline applications but may increase response size significantly.

3. **Performance:** When requesting word images for multiple verses, consider:
   - Using pagination to limit the number of verses per request
   - Caching images on the client side
   - Using the CDN URLs efficiently

4. **Font Rendering:** For applications that want to render text directly with fonts (instead of using images), use the appropriate text field (e.g., `text_indopak`) and install the corresponding font on your client application.

## Example Use Cases

### 1. Word-by-Word Quran Reader with Indo-Pak Font

```bash
# Get chapter 1 with Indo-Pak word images
curl "https://api.quran.com/api/v4/verses/by_chapter/1?words=true&word_fields=text_indopak,image_url,position,audio_url,location"
```

### 2. Page-Based Quran Display with Images

```bash
# Get all verses on page 1 with verse images
curl "https://api.quran.com/api/v4/quran/verses/v1_image?page_number=1"
```

### 3. Verse with Word Highlighting

```bash
# Get specific verse with word positions and images
curl "https://api.quran.com/api/v4/verses/by_key/2:255?words=true&word_fields=text_indopak,image_url,position,location,page_number"
```

## Further Resources

- **API Documentation:** https://api-docs.quran.com/docs/category/quran.com-api
- **Community Support:** Join the Discord community at https://discord.com/invite/FxRWSBfWxn
- **Font Resources:** For font files and additional information about Quran fonts, refer to the Quran.com font repository

## Support

If you have questions or need help implementing these features:
1. Check the full API documentation at https://api-docs.quran.com
2. Join the Quran.com Discord community
3. Open an issue on the GitHub repository
