---
title: My first Blog Post
description: Learning how to use @nuxt/content to create a blog
image: /images/179-800x600.jpg
alt: my first blog post
author:
  name: Benjamin
  bio: All about Benjamin
  image: https://picsum.photos/seed/face/100/100
---
  
<div class="bg-blue-500 text-white p-4 mb-4">
Изображения, включенные в содержимое статьи, всегда следует помещать в папку static, поскольку @nuxt/content не зависит от Webpack. Эта папка не пропускается через Webpack, в отличие от папки assets.
</div>

## This is a heading

This is some more info

### This is a sub heading

This is some more info

### This is another sub heading

This is some more info

## А этот на русском

This is some more info

<info-box>
  <template #info-box>
    Автоматический импорт компонентов из markdown не будет работать, если мы не зарегистрируем их глобально, добавив папку global внутри папки components. 
    <br><br>
    Глобальные компоненты будут доступны для всего нашего приложения, поэтому будьте осторожны при добавлении компонентов в эту папку. Это работает иначе, чем добавление компонентов в папку components, которые добавляются (наверное, имеется в виду импортируются — прим. пер.) только в том случае, если они используются (начиная с Nuxt v2.13 компоненты в папке components импортируются автоматически, достаточно написать в Nuxt конфиге: components: true — прим. пер.).
  </template>
</info-box>
