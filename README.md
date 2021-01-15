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
{% if collection.image %}
	<div class="collection-banner">
		{{ collection.image | img_url: 'medium'}}
	</div>
{% endif %}
```

<b>3. 

```html
<ul class="pagination">

{% unless paginate.previous.is_link %}
	<a>Previous</a>
	{% else %}
	<a href="{{paginate.previous.url}}">Previous</a>
{% endunless %}

<li class="pagination_pages">
	{{ 'general.pagination.current_page' | t: current: paginate.current_page, total: paginate.pages }}
</li>

{% unless paginate.next.is_link %}
	<a>Next</a>
	{% else %}
	<a href="{{paginate.next.url}}">Next</a>
{% endunless %}
</ul>
```

<b>4. 
  
```html
{% if product.title == 'Blue T-shirt" %}

	{{% assign blueshirtId = 123 %}}
	{{% assign blueshirtTitle = Blue T-Shirt %}}
	{{% assign blueshirtHandle = blue-t-shirt %}}
	{{% assign blueshirtUrl = /products/blue-t-shirt %}}
	{{% assign blueshirtImage = 'blueshirtimage.jpg | asset_url %}}

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
