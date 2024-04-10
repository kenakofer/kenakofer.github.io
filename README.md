
Kenan Schaefkofer <a class="pronunciation-button" onclick="play()"><img src="{{ site.baseurl }}/assets/icons/speaker.png"></a> is a software engineer with passions for games, music, math, and open-source software. This page highlights several personal projects built by Kenan over the years, most of which can still be enjoyed today. You can find his professional experience in his
<a href="{{ site.baseurl }}/assets/pdf/Resume_v1.10.pdf" target="_blank">resume</a>.


<script>
    function play() {
        var audio = document.getElementById("pronunciation-audio");
        audio.play();
    }
</script>
<audio id="pronunciation-audio" src="{{ site.baseurl }}/assets/audio/pronunciation.mp3"></audio>

### Personal Projects

{% assign sorted = site.data.projects | sort %}
{% for project_hash in sorted %}
{% assign project = project_hash[1] %}
{% unless project.id == "hide" %}
<div class="project-card" data-id="{{ project.id }}">
    <div class="card-year">{{ project.date }}</div>
    <div class="card-left">
        <img class="project-thumb" src="{{ site.baseurl }}/assets/img/{{ project.screenshot }}">
        <video class="project-vid" data-id="{{ project.id }}" loop muted playsinline>
            <source src="{{ site.baseurl }}/assets/mp4/{{ project.mp4 }}">
        </video>
    </div>
    <div class="card-right">
        <span class="project-title">{{ project.title }}</span>
        <p>{{ project.description }} <a href="{{ project.github-link }}">{{ project.github-text }}</a></p>
    </div>
</div>
{% endunless %}
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