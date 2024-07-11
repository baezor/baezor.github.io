---
layout: post
title: Automating Post Creation on Jekyll with a Custom Bash Script
date: 2024-07-10 00:00:00 -0500
description: A bash script to automate the creation of new posts on Jekyll.
categories: jekyll bash
author: Angel Baez
---

I wrote a bash script to automate the creation of new posts on Jekyll.

Here's the script:

```bash
#!/bin/bash

# Function to slugify the title
slugify() {
    echo "$1" | iconv -t ascii//TRANSLIT | tr -cd '[:alnum:] [:space:]' | tr '[:upper:]' '[:lower:]' | tr ' ' '-' | tr -s '-'
}

# Get the current date
YEAR=$(date +%Y)
MONTH=$(date +%m)
DAY=$(date +%d)
CURRENT_DATE=$(date +"%Y-%m-%d %H:%M:%S %z")

# Request the post's title
echo "Enter the title of your post:"
read TITLE

# Slugify the title
SLUG=$(slugify "$TITLE")

# Create the filename
FILENAME="${YEAR}-${MONTH}-${DAY}-${SLUG}.md"

# Define the folder
FOLDER="_posts"

# Create the folder if it does not exist
mkdir -p "$FOLDER"

# Create the new file with the specified structure in the _posts folder
cat <<EOL > "${FOLDER}/${FILENAME}"
---
layout: post
title: $TITLE
date: $CURRENT_DATE
description:
categories:
author:
---

# Welcome

Start writing your post
EOL

echo "File '${FOLDER}/${FILENAME}' created successfully."
```

Hope you find it useful if you're using Jekyll for your blog!

See you in the next post!
