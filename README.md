
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
<div id="year-parallax" aria-hidden="true"></div>
<div id="projects">
{% for project_hash in sorted %}
{% assign project = project_hash[1] %}
{% unless project.id == "hide" %}
<div class="project-card" data-id="{{ project.id }}" data-year="{{ project.date }}">
    <div class="card-left">
        <img class="project-thumb" src="{{ site.baseurl }}/assets/img/{{ project.screenshot }}">
        {% if project.mp4 != "" %}
        <video class="project-vid" data-id="{{ project.id }}" loop muted playsinline>
            <source src="{{ site.baseurl }}/assets/mp4/{{ project.mp4 }}">
        </video>
        {% endif %}
    </div>
    <div class="card-right">
        <span class="project-title">{{ project.title }}</span>
        <p>{{ project.description }} <a href="{{ project.github-link }}">{{ project.github-text }}</a></p>
    </div>
</div>
{% endunless %}
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
    // Evenly-spaced parallax years drifting behind the project cards.
    (function () {
        var layer = document.getElementById("year-parallax");
        var projects = document.getElementById("projects");
        if (!layer || !projects) return;

        // Distinct years in card order (already sorted newest-first).
        var years = [];
        document.querySelectorAll(".project-card[data-year]").forEach(function (card) {
            var y = card.getAttribute("data-year");
            if (y && years.indexOf(y) === -1) years.push(y);
        });

        // Fraction of the page's scroll speed the year layer moves at.
        // < 1 => years drift slower than the cards (classic parallax).
        var SPEED = 0.6;

        var labels = years.map(function (y) {
            var el = document.createElement("span");
            el.className = "year-mark";
            el.textContent = y;
            layer.appendChild(el);
            return el;
        });

        function layout() {
            // Anchor each year evenly, but packed into a compressed band so the
            // labels sit close together and every year stays visible while the
            // slower parallax drags the layer upward.
            var top = projects.offsetTop;
            var height = projects.offsetHeight;
            // Compress the labels into the middle SPREAD fraction of the section.
            var SPREAD = 0.5;
            var usable = height * SPREAD;
            var start = top + (height - usable) / 2;
            var n = labels.length;
            // Fixed layer => align to the viewport-left of the content column.
            var colLeft = projects.getBoundingClientRect().left;
            labels.forEach(function (el, i) {
                var frac = n > 1 ? i / (n - 1) : 0.5;
                el.dataset.baseTop = Math.round(start + usable * frac);
                el.style.left = Math.round(colLeft - 8) + "px";
            });
            onScroll();
        }

        function onScroll() {
            var scrollY = window.pageYOffset || document.documentElement.scrollTop;
            // Layer is position:fixed, so `top` is the on-screen position.
            // A card anchored at `base` is on screen at (base - scrollY); moving
            // the year at (base - scrollY * SPEED) makes it lag behind => parallax.
            labels.forEach(function (el) {
                var base = parseFloat(el.dataset.baseTop) || 0;
                el.style.top = (base - scrollY * SPEED) + "px";
            });
        }

        window.addEventListener("scroll", onScroll, { passive: true });
        window.addEventListener("resize", layout);
        // Recompute once images have loaded and shifted the layout.
        window.addEventListener("load", layout);
        layout();
    })();
</script>