
# Projects
Here are some of the past and present projects that consume my time.

{% for project in site.projects %}
  <h3>
    <a href="{{ project.url }}">
      {{ project.name }}
    </a>
    <p>{{ project.description }}</p>
  </h3>
{% endfor %}