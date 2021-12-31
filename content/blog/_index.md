---
title: Blog
description: |
  Um blog com categorias, 
  tags, series, e paginação.
author: "R. Accioly"
show_post_thumbnail: true
show_author_byline: true
show_post_date: true
# for listing page layout
layout: list-sidebar # list, list-sidebar, list-grid

# for list-sidebar layout
sidebar: 
  title: Alguns comentários
  description: |
    Esta tarefa de construir um blog é um pouco espinhosa, mas aos poucos os resultados vão surgindo!
    
#    Check out the _index.md file in the /blog folder 
#    to edit this content. 
  author: "R.Accioly"
#  text_link_label: Subscreva via RSS
  text_link_url: /index.xml
  show_sidebar_adunit: false # show ad container

# set up common front matter for all pages inside blog/
cascade:
  author: "R. Accioly"
  show_author_byline: true
  show_post_date: true
  show_disqus_comments: false # see disqusShortname in site config
  # for single-sidebar layout
  sidebar:
    text_link_label: Veja os posts recentes
    text_link_url: /blog/
    show_sidebar_adunit: false # show ad container
---

** No content below YAML for the blog _index. This file provides front matter for the listing page layout and sidebar content. It is also a branch bundle, and all settings under `cascade` provide front matter for all pages inside blog/. You may still override any of these by changing them in a page's front matter.**
