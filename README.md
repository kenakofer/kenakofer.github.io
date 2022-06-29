
Kenan Schaefkofer <a class="pronunciation-button" onclick="play()"><img src="{{ site.baseurl }}/assets/icons/speaker.png"></a> is a software engineer with passions for games, music, math, and improving discourse. This page highlights personal, open-source projects built by Kenan over the years, most of which can still be enjoyed today. You can find his professional experience and [resume](TODO) over on this [other page](TODO).

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
<div class="project-card" data-id="{{ project.id }}">
<div class="card-left">
<img class="project-thumb" src="{{ site.baseurl }}/assets/img/{{ project.screenshot }}">
<video class="project-vid" data-id="{{ project.id }}" loop muted playsinline>
    <source src="{{ site.baseurl }}/assets/mp4/{{ project.mp4 }}">
</video>
</div>
<div class="card-right">
<span class="project-title">{{ project.title }}</span>
<p>
{{ project.description }} <a href="{{ project.github-link }}">{{ project.github-text }}</a>
</p>
</div>
</div>
{% endfor %}

<script>
    var cards = document.querySelectorAll(".project-card");
    cards.forEach(function(card) {
        card.addEventListener("mouseenter", function() {
            data_id = event.target.getAttribute('data-id');
            if (!data_id) return;
            vid = document.querySelector('.project-vid[data-id="'+data_id+'"]');
            vid.play();
        });
        card.addEventListener("mouseleave", function() {
            data_id = event.target.getAttribute('data-id');
            if (!data_id) return;
            vid = document.querySelector('.project-vid[data-id="'+data_id+'"]');
            vid.pause();
        });
    });
</script>