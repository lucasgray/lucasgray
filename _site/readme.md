# Hello!

My name is Lucas. I'm a guy that likes building software - both at home and at work (UPDATE Sept 2020 - this used to be a useful distinction).

Here are some of the past and present projects that consume my time.

{% for project in site.projects %}
  <h3>
    <a href="{{ project.url }}">
      {{ project.name }}
    </a>
    <p>{{ project.description }}</p>
  </h3>
{% endfor %}