## Welcome to Project A

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  {% for post in category[1] %}
    <article>
      {{ post.content }}
      - <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_long_string }}</time>
    </article>
  {% endfor %}
{% endfor %}

END
