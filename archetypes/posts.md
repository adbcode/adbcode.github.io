---
layout: post
title: '{{ replaceRE `^\d{4}-\d{2}-\d{2}-` "" .File.ContentBaseName | replaceRE "-" " " | title }}'
draft: true
---
