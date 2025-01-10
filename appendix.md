---
layout: default
permalink: /appendix/
title: Appendix
class: license-types
---

For reference, here is a table of every license described in the [choosealicense.com repository](https://github.com/github/choosealicense.com).

If you're here to choose a license, **[start from the home page](/)** to see a few licenses that will work for most cases.

<table border style="font-size: xx-small; position: relative">
{% assign types = "permissions|conditions|limitations" | split: "|" %}
<tr style="position: sticky; top: 0; z-index: 1000001; background: color-mix(in srgb, var(--backgroundColor) 70%, transparent);">
  <th scope="col" style="text-align: center">License</th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% if seen_tags contains rule_obj.tag or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:rule_obj.tag }}{% endcapture %}
      <th scope="col" style="text-align: center; width:7%"><a href="#{{ rule_obj.tag }}">{{ rule_obj.label }}</a></th>
    {% endfor %}
  {% endfor %}
</tr>
{% assign licenses = site.licenses | sort: "path" %}
{% for license in licenses %}
  <tr style="height: 3em"><th scope="row"><a href="{{ license.id }}">{{ license.title }}</a></th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% assign req = rule_obj.tag %}
      {% if seen_tags contains req  or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
      {% assign seen_req = false %}
      {% for t in types %}
        {% for r in license[t] %}
          {% if r contains req %}
            <td class="license-{{ t }}" style="text-align:center">
              {% if r contains "--" %}
                {% assign lite = " lite" %}
              {% else %}
                {% assign lite = "" %}
              {% endif %}
              <span class="{{ r | append: lite }}" style="margin: auto;">
                <span class="license-sprite {{ r }}"></span>
              </span>
            </td>
            {% assign seen_req = true %}
          {% endif %}
        {% endfor %}
      {% endfor %}
      {% unless seen_req %}
        <td></td>
      {% endunless %}
    {% endfor %}
  {% endfor %}
  </tr>
{% endfor %}
</table>

## Legend

<p>Open source licenses grant to the public <span class="license-permissions"><span class="license-sprite"></span></span> <b>permissions</b> to do things with licensed works which copyright or other "intellectual property" laws might otherwise disallow.</p>

<p>Most open source licenses' grants of permissions are subject to compliance with <span class="license-conditions"><span class="license-sprite"></span></span> <b>conditions</b>.</p>

<p>Most open source licenses also have <span class="license-limitations"><span class="license-sprite"></span></span> <b>limitations</b> that usually disclaim warranty and liability, and sometimes expressly exclude patents or trademarks from licenses' grants.</p>

{% for type in types %}
### {% if type == "permissions" %}Permissions{% elsif type == "conditions" %}Conditions{% else %}Limitations{% endif %}
  <dl>
  {% assign rules = site.data.rules[type] | sort: "label" %}
  {% for rule_obj in rules %}
    {% assign req = rule_obj.tag %}
    <dt id="{{ req }}">{{ rule_obj.label }}</dt>
    <dd class="license-{{ type }}">
      {% if req contains "--" %}
        {% assign lite = " lite" %}
      {% else %}
        {% assign lite = "" %}
      {% endif %}
      <span class="{{ req | append: lite }}">
        <span class="license-sprite {{ req }}"></span>
      </span>
      {{ rule_obj.description }}
    </dd>
  {% endfor %}
  </dl>
{% endfor %}
