---
layout: page
title: Algorithm
algo_menubar: algo_menu
hero_height: is-small
hero_image: /assets/img/peach-camellia.jpeg
show_sidebar: false
---

## Algorithm

Collection of algorithm problem solving.

#### Index

{% assign menus = site.data.[page.algo_menubar] %}
{% assign everything = [] %}
{% for menu in menus %}
    {% for item in menu.items %}
        {% assign submenus = site.data.[item.list] %}
        {% for submenu in submenus %}
            {% assign everything = everything | concat: submenu.items %}
        {% endfor %}
    {% endfor %}
{% endfor %}

{% assign everything = everything | sort: 'name' %}
<div class="menu">
    <dl class="menu-list">
    {% for item in everything %}
        <dt class="is-size-6">
            <a href="{{ item.link | relative_url }}">
                {{ item.name }}
            </a>
        </dt>
    {% endfor %}
    </dl>
</div>
