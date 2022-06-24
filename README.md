
Kenan Schaefkofer <a class="pronunciation-button" onclick="play()">pronunciation</a> is a software engineer with passions for games, music, math, and improving discourse. This page highlights personal, open-source projects built by Kenan over the years, most of which can still be enjoyed today. You can find his professional experience and [resume](TODO) over on this [other page](TODO).

<script>
    function play() {
        var audio = document.getElementById("pronunciation-audio");
        audio.play();
    }
</script>
<audio id="pronunciation-audio" src="{{ site.baseurl }}/assets/audio/pronunciation.mp3"></audio>

### Personal Projects

{% for project_hash in site.data.projects %}
{% assign project = project_hash[1] %}
<div class="project-card">
<div class="card-left">
<img class="project-thumb" src="{{ site.baseurl }}/assets/img/{{ project.screenshot }}">
</div>
<div class="card-right">
<span class="project-title">{{ project.title }}</span>
<p>
{{ project.description }} <a href="{{ project.github-link }}">{{ project.github-text }}</a>
</p>
</div>
</div>
{% endfor %}
