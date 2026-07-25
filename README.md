
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
{% comment %} Only visible projects, so rows pair up correctly. {% endcomment %}
{% assign visible = "" | split: "" %}
{% for project_hash in sorted %}
{% assign project = project_hash[1] %}
{% unless project.id == "hide" %}{% assign visible = visible | push: project %}{% endunless %}
{% endfor %}
<div id="projects">
{% for project in visible %}
{% assign col = forloop.index0 | modulo: 2 %}
{% if col == 0 %}<div class="project-row">{% endif %}
<div class="project-card" data-id="{{ project.id }}">
    <div class="card-year">{{ project.date }}</div>
    <a class="card-left" href="{{ project.github-link }}">
        <img class="project-thumb" src="{{ site.baseurl }}/assets/img/{{ project.screenshot }}">
        {% if project.mp4 != "" %}
        <video class="project-vid" data-id="{{ project.id }}" loop muted playsinline>
            <source src="{{ site.baseurl }}/assets/mp4/{{ project.mp4 }}">
        </video>
        {% endif %}
    </a>
    <div class="card-right">
        <a class="project-title" href="{{ project.github-link }}">{{ project.title }}</a>
        <p>{{ project.description }} <a href="{{ project.github-link }}">{{ project.github-text }}</a></p>
    </div>
</div>
{% if col == 1 or forloop.last %}</div>{% endif %}
{% endfor %}
</div>

<script>
    var cards = document.querySelectorAll(".project-card");
    cards.forEach(function(card) {
        card.addEventListener("mouseenter", function() {
            data_id = event.target.getAttribute('data-id');
            if (!data_id) return;
            vid = document.querySelector('.project-vid[data-id="'+data_id+'"]');
            if (!vid) return;
            vid.play();
        });
        card.addEventListener("mouseleave", function() {
            data_id = event.target.getAttribute('data-id');
            if (!data_id) return;
            vid = document.querySelector('.project-vid[data-id="'+data_id+'"]');
            if (!vid) return;
            vid.pause();
        });
    });
</script>

<script>
    // In a two-card row, let the wordier card claim more horizontal space from
    // its partner. Outer edges stay grid-aligned; the center seam goes ragged.
    (function () {
        var WIDE = window.matchMedia("(min-width: 900px)");

        function balanceRow(row) {
            var cards = row.querySelectorAll(".project-card");
            if (cards.length !== 2) return;            // lone last card: leave as-is
            // Measure each card's content demand at an equal 50/50 split.
            row.style.gridTemplateColumns = "1fr 1fr";
            var a = cards[0].querySelector(".card-right");
            var b = cards[1].querySelector(".card-right");
            // scrollHeight of the text column ~ how much room the words want.
            var da = a ? a.scrollHeight : cards[0].scrollHeight;
            var db = b ? b.scrollHeight : cards[1].scrollHeight;
            if (!da || !db) return;
            // Fully proportional split, clamped so neither card collapses.
            var fa = da / (da + db);
            fa = Math.max(0.3, Math.min(0.7, fa));
            row.style.gridTemplateColumns = fa.toFixed(4) + "fr " + (1 - fa).toFixed(4) + "fr";
        }

        function balanceAll() {
            var rows = document.querySelectorAll(".project-row");
            rows.forEach(function (row) {
                if (WIDE.matches) {
                    balanceRow(row);
                } else {
                    row.style.gridTemplateColumns = "";   // single column when narrow
                }
            });
        }

        window.addEventListener("resize", balanceAll);
        window.addEventListener("load", balanceAll);       // after images settle layout
        (WIDE.addEventListener ? WIDE.addEventListener("change", balanceAll)
                              : WIDE.addListener(balanceAll));
        balanceAll();
    })();
</script>