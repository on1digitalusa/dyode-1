# dyode-1
<b> 1. 
Locate appropriate theme file in (Sections); place where ever desired for customizer. Then make sure to add {% schema %} markup
 
```html
{% if section.settings.text != blank %}
	<div class="edit-text">
		{{ section.settings.text }}
	</div>
{% endif %}

{% schema %}    {
      "type": "richtext",
      "id": "text",
      "label": {
        "en": "Text"
      },
      "default": {
        "en": "<p>Enter your editable text here.</p>"
      }
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
