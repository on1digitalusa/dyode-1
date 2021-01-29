# dyode-1
<b> 1. 
Add the theme file in (Sections); place where ever desired for customizer. In the example below I have taken it a step further and added in the option to change the background-color; and color of the text for the cutomizer. 
 
```html
{% if section.settings.divider %}<div class="section--divider">{% endif %}

<div
  data-section-id="{{ section.id }}"
  data-section-type="featured-content-section"
  class="text-{{ section.settings.align_text }} section-id-{{ section.id }}" style="padding-top: {{ section.settings.top-space }}%; padding-bottom: {{ section.settings.bottom-space }}%;">

  <div class="page-width">
    <div class="grid">
      <div class="grid__item{% if section.settings.narrow_column %} medium-up--three-quarters medium-up--push-one-eighth{% endif %}">
        {% if section.settings.title != blank %}
          <h2>{{ section.settings.title }}</h2>
        {% endif %}

        {% if section.settings.text != blank %}
          <div class="rte">
            {% if section.settings.enlarge_text %}<div class="enlarge-text">{% endif %}
            {{ section.settings.text }}
            {% if section.settings.enlarge_text %}</div>{% endif %}
          </div>
        {% endif %}

        {% for block in section.blocks %}
          <div class="rte" {{ block.shopify_attributes }}>
            {% case block.type %}
              {% when 'page' %}
                {% if block.settings.home_page_content != blank %}
                  {{ pages[block.settings.home_page_content].content }}
                {% else %}
                  {{ 'home_page.onboarding.no_content' | t }}
                {% endif %}
              {% when 'text' %}
                {% if block.settings.home_page_richtext != blank %}
                  {% if block.settings.enlarge_text %}<div class="enlarge-text">{% endif %}
                  {{ block.settings.home_page_richtext }}
                  {% if block.settings.enlarge_text %}</div>{% endif %}
                {% else %}
                  {{ 'home_page.onboarding.no_content' | t }}
                {% endif %}
            {% endcase %}
          </div>
        {% endfor %}
      </div>
    </div>
  </div>
</div>
<style>
  .section-id-{{ section.id }}{
    background-color: {{ section.settings.section_bg }};
  }
  .section-id-{{ section.settings.section.id }} h2{
    color: {{ section.settings.section_title_color }};
  }
  .section-id-{{ section.settings.section.id }} .enlarge-text p{
    color: {{ section.settings.section_text_color }};
  }
  .section-id-{{ section.settings.section.id }} .enlarge-text a{
    color: {{ section.settings.section_link_color }};
  }
  .no-margin {
    margin: 0 auto;
}
</style> 
{% if section.settings.divider %}</div>{% endif %}

{% schema %}
  {
    "name": "Rich text",
    "class": "index-section no-margin",
    "max_blocks": 2,
    "settings": [
      {
        "type": "range",
        "id": "top-space",
        "label": "Top Space",
		"max": 20,
		"min": 0,
		"step": 1,
		"unit": "%",
		"default": 2
      },
      {
        "type": "range",
        "id": "bottom-space",
        "label": "Bottom Space",
		"max": 20,
		"min": 0,
		"step": 1,
		"unit": "%",
		"default": 2
      },
      {
        "type": "text",
        "id": "title",
        "label": "Heading"
      },
      {
        "type": "richtext",
        "id": "text",
        "label": "Text",
        "default": "<p>A sentence or two introducing your brand, what you sell, and what makes your brand compelling to customers.</p>"
      },
      {
        "type": "select",
        "id": "align_text",
        "label": "Text alignment",
        "default": "center",
        "options": [
          {
            "value": "left",
            "label": "Left"
          },
          {
            "value": "center",
            "label": "Centered"
          }
        ]
      },
      {
        "type": "checkbox",
        "id": "narrow_column",
        "label": "Narrow column"
      },
      {
        "type": "checkbox",
        "id": "enlarge_text",
        "label": "Enlarge text",
        "default": true
      },
      {
        "type": "checkbox",
        "id": "divider",
        "label": "Show section divider",
        "default": false
      },
      {
        "type": "color",
        "id": "section_bg",
        "label": "Section Background",
        "default": "#DDD"
      },
      {
        "type": "color",
        "id": "section_title_color",
        "label": "Section Title Color",
        "default": "#000"
      },
      {
        "type": "color",
        "id": "section_text_color",
        "label": "Section Text Color",
        "default": "#000"
      },
      {
        "type": "color",
        "id": "section_link_color",
        "label": "Rich Text Link Color",
        "default": "#3b958f"
      }
    ],
    "presets": [{
      "name": "Rich text",
      "category": "Text"
    }],
    "blocks" : [
      {
        "type": "text",
        "name": "Text",
        "settings": [
          {
            "type": "checkbox",
            "id": "enlarge_text",
            "label": "Enlarge text",
            "default": false
          },
          {
            "id": "home_page_richtext",
            "type": "richtext",
            "label": "Text",
            "default": "<p>Use this text to share information about your brand with your customers. Describe a product, share announcements, or welcome customers to your store.</p>"
          }
        ]
      },
      {
        "type": "page",
        "name": "Page",
        "settings": [
          {
            "id": "home_page_content",
            "type": "page",
            "label": "Page"
          }
        ]
      }
    ]
  }
{% endschema %}


```

<b>2. Use the snippet below to check for the image; then load via shopify admin 

```html
{% if section.settings.parallax %}data-parallax="true"{% endif %}> 
{%- if section.settings.collection_image_enable and collection.image -%}
    <div class="collection-hero loading--delayed">
      {%- if section.settings.parallax -%}
        <div class="parallax-container">
          <div
            class="parallax-image collection-hero__image lazyload"
            data-bgset="{% include 'bgset', image: collection.image %}"
            data-sizes="auto">
          </div>
        </div>
      {%- else -%}
        {%- assign img_url = collection.image | img_url: '1x1' | replace: '_1x1.', '_{width}x.' -%}
        <img class="collection-hero__image image-fit lazyload"
          src=""
          data-src="{{ img_url }}"
          data-aspectratio="{{ collection.image.aspect_ratio }}"
          data-sizes="auto"
          data-parent-fit="cover"
          alt="{{ collection.image.alt | escape }}">
        <noscript>
          <img class="collection-hero__image image-fit"
            src="{{ collection.image | img_url: '1400x' }}"
            alt="{{ collection.image.alt | escape }}">
        </noscript>
      {%- endif -%}

      <div class="collection-hero__content">
        <div class="page-width">
          <header class="section-header section-header--hero">
            <h1 class="section-header__title section-header__title--medium">
              <div class="animation-cropper">
                <div class="animation-contents">
                  {{ collection.title }}
                </div>
              </div>
            </h1>
          </header>
        </div>
      </div>
    </div>
```

<b>3. 
Custom pagination. Add assets, 'icon-chevron-left', and 'icon-chevron-right'
	
	```html
<div id="pagination" class="clearfix">
    <ul class="pagination">
        {% if paginate.previous %}
            <li class="pagination_previous">
                <a href="{{ paginate.previous.url }}" title="{{ paginate.previous.title }}">
                   {% include 'icon-chevron-left' %}
                </a>
            </li>
        {% else %}
            <li class="pagination_previous disabled">
                <span>
                    {% include 'icon-chevron-left' %}
                </span>
            </li>
        {% endif %}
        {% for part in paginate.parts %}
            {% if part.is_link %}
                <li>
                    <a href="{{ part.url }}" title="">{{ part.title }}</a>
                </li>
            {% else %}
                {% if part.title == paginate.current_page %}
                    <li class="active"><span>{{ part.title }}</span></li>
                {% else %}
                    <li><span>{{ part.title }}</span></li>
                {% endif %}
            {% endif %}
        {% endfor %}
        {% if paginate.next %}
            <li class="pagination_next">
                <a href="{{ paginate.next.url }}" title="{{ paginate.next.title }}">
                {% include 'icon-chevron-right' %}
                </a>
            </li>
        {% else %}
            <li class="pagination_next disabled">
                <span>
                {% include 'icon-chevron-right' %}
                </span>
            </li>
        {% endif %}
    </ul>
</div>
{% if paginate.pages > 1 %}
<div class="clearfix">
  <a href="{{collection.url}}" class="view-all-collection">View All</a>
</div>
{% endif %}

<b>4. 
  
```html
{% if product.title == 'Blue T-shirt" %}

	{{% assign blueshirtId = 123 %}}
	{{% assign blueshirtTitle = Blue T-Shirt %}}
	{{% assign blueshirtHandle = blue-t-shirt %}}
	{{% assign blueshirtUrl = /products/blue-t-shirt %}}
	{{% assign blueshirtImage = "blue-t-shirt.jpg" | asset_url %}}

{% endif %}
```

<b>5.

```html
{% assign dyodeArray = "fruit, vegetable, cloth, denim" | split: ',' %}

{% for key in dyodeArray %}
	{% if key == 'fruit' %}
		{% assign fruit = "apple" %}
	{% else %}
	{% if key == 'vegetable' %}
		{% assign vegetable = "carrot" %}
	{% else %}
	{% if key == 'cloth' %}
		{% assign cloth = "t-shirt" %}
	{% else %}
	{% if key == 'denim' %}
		{% assign denim = "jeans" %}
	{% endif %}
{% endfor %}

<b>Section-2
	
Please see - https://codepen.io/on1digital/live/XWjwpea
Please see - https://gist.github.com/on1digitalusa/ce6090663795b7fbbc7880487d318940
	```html








