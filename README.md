<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Space Explorer: From Sun to Galaxies</title>
    <style>
        :root {
            --overlay-alpha: 0.55;
            --accent: #4fc3f7;
            --glass: rgba(0,0,0,0.55);
            --radius: 14px;
        }

        /* Reset */
        * { box-sizing: border-box; margin: 0; padding: 0; }
        html,body { height: 100%; scroll-behavior: smooth; font-family: Arial, Helvetica, sans-serif; background: #000; color: #fff; }

        /* Sections */
        .space-section {
            position: relative;
            height: 100vh;
            padding-top: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            background-attachment: fixed; /* parallax on desktop */
            border-bottom: 1px solid rgba(255,255,255,0.03);
        }

        /* dark overlay to keep text readable */
        .space-section::before {
            content: "";
            position: absolute;
            inset: 0;
            background: rgba(0,0,0,var(--overlay-alpha));
            transition: background 0.3s ease;
            z-index: -1;
        }

        .content {
            position: relative;
            z-index: 1;
            max-width: 900px;
            margin: 1rem;
            padding: 2rem;
            background: linear-gradient(180deg, rgba(0,0,0,0.45), rgba(0,0,0,0.6));
            border-radius: var(--radius);
            box-shadow: 0 8px 30px rgba(0,0,0,0.6);
            backdrop-filter: blur(6px);
            opacity: 0.1;
        }
        .content:hover {
            opacity: 1;
            transition: opacity 0.4s ease;
           }

        

        h1 { font-size: 2.6rem; color: var(--accent); margin-bottom: 0.6rem; }
        p { font-size: 1.05rem; line-height: 1.6; margin-bottom: 0.8rem; color: #e6eef6; }

        .facts { display:flex; gap:12px; flex-wrap:wrap; justify-content:center; margin-top:0.6rem; }
        .fact { background: rgba(255,255,255,0.04); padding: 8px 12px; border-radius: 10px; font-size:0.95rem; }

        .planet-nav { position: fixed; top: 0; left: 0; right: 0; z-index: 50; background: transparent; padding: 5px; display:flex; gap:8px; flex-wrap:wrap; justify-content:center; }

        .btn {
            display:inline-block;
            margin-top: 14px;
            padding: 10px 18px;
            background: var(--accent);
            color: #002;
            text-decoration:none;
            border-radius: 999px;
            font-weight:700;
            border: none;
            cursor:pointer;
        }

        .nav-btn {
            display:inline-block;
            padding: 5px 10px;
            background: transparent;
            color: #fff;
            text-decoration:none;
            border-radius: 999px;
            font-weight:700;
            font-size: 0.8rem;
            border: 1px solid rgba(255,255,255,0.3);
            cursor:pointer;
            text-shadow: 0 0 5px rgba(0,0,0,0.8);
        }
        .nav-btn:hover {
            background: rgba(255,255,255,0.1);
        }
        

        .scroll-hint { display:block; margin-top:12px; color:#cfeffb; opacity:0.9; animation: bounce 2s infinite; font-size:0.95rem; }

        @keyframes bounce { 0%,100%{ transform: translateY(0);} 50%{ transform: translateY(8px);} }

        /* compact cards for stars list */
        .star-grid { display:grid; gap:12px; grid-template-columns: repeat(auto-fit,minmax(220px,1fr)); margin-top:12px; }
        .star-card { background: rgba(255,255,255,0.04); padding:12px; border-radius:10px; text-align:left; }
        .muted { color: #c3dbe8; font-size:0.95rem; }

        /* top progress dots */
        .dots {
            position: fixed;
            left: 18px;
            top: 50%;
            transform: translateY(-50%);
            z-index: 30;
            display:flex;
            flex-direction:column;
            gap:10px;
        }
        .dot {
            width:12px; height:12px; border-radius:50%;
            background: rgba(255,255,255,0.18); cursor:pointer; border:2px solid rgba(255,255,255,0.06);
        }
        .dot.active { background: var(--accent); box-shadow:0 0 12px rgba(79,195,247,0.28); }

        /* Comet styling */
        .comet {
            position: fixed;
            pointer-events: none;
            z-index: 20;
        }

        .comet-trail {
            position: fixed;
            pointer-events: none;
            z-index: 19;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(255,255,255,0.8), rgba(0,150,255,0.3));
            box-shadow: 0 0 20px 5px rgba(0,150,255,0.4);
        }

        /* responsive */
        @media (max-width:720px) {
            h1 { font-size: 1.8rem; }
            .space-section { background-attachment: scroll; }
        }
    </style>
</head>
<body>
    <div class="planet-nav">
        <a class="nav-btn" href="#mercury">Mercury</a>
        <a class="nav-btn" href="#venus">Venus</a>
        <a class="nav-btn" href="#earth">Earth</a>
        <a class="nav-btn" href="#mars">Mars</a>
        <a class="nav-btn" href="#jupiter">Jupiter</a>
        <a class="nav-btn" href="#saturn">Saturn</a>
        <a class="nav-btn" href="#uranus">Uranus</a>
        <a class="nav-btn" href="#neptune">Neptune</a>
        <a class="nav-btn" href="#kuiper-belt">Kuiper Belt</a>
        <a class="nav-btn" href="#stars">Stars</a>
        <a class="nav-btn" href="#milkyway">Milkyway</a>
        <a class="nav-btn" href="#black-hole">Black hole</a>
        <a class="nav-btn" href="#galaxies">Galaxies</a>

    </div>

    <div class="dots" id="navDots" aria-hidden="true"></div>

    <img class="comet" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Cdefs%3E%3ClinearGradient id='cometGrad' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' style='stop-color:white;stop-opacity:1'/%3E%3Cstop offset='100%25' style='stop-color:cyan;stop-opacity:0.3'/%3E%3C/linearGradient%3E%3C/defs%3E%3Ccircle cx='50' cy='50' r='6' fill='white' filter='drop-shadow(0 0 8px white)'/%3E%3Cline x1='50' y1='50' x2='50' y2='0' stroke='url(%23cometGrad)' stroke-width='2' opacity='0.8'/%3E%3C/svg%3E" alt="comet" width="30" height="30">
    <img class="comet" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Cdefs%3E%3ClinearGradient id='cometGrad' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' style='stop-color:white;stop-opacity:1'/%3E%3Cstop offset='100%25' style='stop-color:cyan;stop-opacity:0.3'/%3E%3C/linearGradient%3E%3C/defs%3E%3Ccircle cx='50' cy='50' r='6' fill='white' filter='drop-shadow(0 0 8px white)'/%3E%3Cline x1='50' y1='50' x2='50' y2='0' stroke='url(%23cometGrad)' stroke-width='2' opacity='0.8'/%3E%3C/svg%3E" alt="comet" width="30" height="30">
    <img class="comet" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Cdefs%3E%3ClinearGradient id='cometGrad' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' style='stop-color:white;stop-opacity:1'/%3E%3Cstop offset='100%25' style='stop-color:cyan;stop-opacity:0.3'/%3E%3C/linearGradient%3E%3C/defs%3E%3Ccircle cx='50' cy='50' r='6' fill='white' filter='drop-shadow(0 0 8px white)'/%3E%3Cline x1='50' y1='50' x2='50' y2='0' stroke='url(%23cometGrad)' stroke-width='2' opacity='0.8'/%3E%3C/svg%3E" alt="comet" width="30" height="30">
    

    <!-- Sun -->
    <section class="space-section" id="sun" style="background-image: url('https://theplanets.org/123/2022/03/What-Is-the-Sun.jpg');">
        <div class="content">
            <h1>The Sun</h1>
            <p>5 Facts About the Sun - Earth HowThe Sun is a 4.6-billion-year-old G2V yellow dwarf star at the center of our solar system, comprising 99.86% of its mass. It is a massive, hot, almost perfect sphere of plasma (mostly hydrogen and helium) powered by nuclear fusion. The Sun is roughly 93 million miles (149 million km) from Earth, and its gravity controls the orbits of everything in our solar system.</p>
            <div class="facts">
                <div class="fact">Type: G2V</div>
                <div class="fact">Radius: ~696,000 km</div>
                <div class="fact">Age: ~4.6 billion years</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Sun" target="_blank" rel="noopener noreferrer">Sun - Wikipedia</a></div>
            </div>
            <span class="scroll-hint">Scroll down to explore the planets ↓</span>
        </div>
    </section>

    <!-- Mercury -->
    <section class="space-section" id="mercury" style="background-image: url('https://images.unsplash.com/photo-1462331940025-496dfbfc7564?auto=format&fit=crop&w=1920&q=80');">
        <div class="content">
            <h1>Mercury</h1>
            <p>Mercury is the smallest planet in our solar system and the closest to the Sun, orbiting it every 88 Earth days. It is a rocky, cratered world with extreme surface temperatures, ranging from roughly -180°C (-290°F) at night to 430°C (800°F) during the day. It has a huge iron core and almost no atmosphere, resembling Earth’s moon. </p>
            <div class="facts">
                <div class="fact">Orbital period: 88 days</div>
                <div class="fact">Surface: Rocky, cratered</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Mercury_(planet)" target="_blank" rel="noopener noreferrer">Mercury - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Venus -->
    <section class="space-section" id="venus" style="background-image: url('https://cdn.mos.cms.futurecdn.net/RifjtkFLBEFgzkZqWEh69P.jpg');">
        <div class="content">
            <h1>Venus</h1>
            <p>Venus is the second planet from the Sun and Earth's closest, similarly sized neighbor, often called Earth's twin. It is the hottest planet in the solar system—with surface temperatures around hot enough to melt lead—due to a thick, toxic carbon dioxide atmosphere that creates a intense runaway greenhouse effect. </p>
            <div class="facts">
                <div class="fact">Surface Temp: ~465°C</div>
                <div class="fact">Atmosphere: Dense CO2</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Venus" target="_blank" rel="noopener noreferrer">Venus - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Earth -->
    <section class="space-section" id="earth" style="background-image: url('https://images.unsplash.com/photo-1614730321146-b6fa6a46bcb4?auto=format&fit=crop&w=1920&q=80');">
        <div class="content">
            <h1>Earth</h1>
            <p>Earth is the third planet from the Sun and the only known world to host life, formed approximately 4.5 billion years ago. It is the largest terrestrial planet in our solar system, with a radius of roughly 3,959 miles. The planet's surface is 71% water and 29% land, characterized by a thin atmosphere, active plate tectonics, and a protective magnetic field.</p>
            <div class="facts">
                <div class="fact">Moon: 1</div>
                <div class="fact">Atmosphere: N2/O2</div>
                <div class="fact">Radius: ~6,371 km</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Earth" target="_blank" rel="noopener noreferrer">Earth - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Mars -->
    <section class="space-section" id="mars" style="background-image: url('https://starwalk.space/gallery/images/mars-red-planet/1140x641.jpg');">
        <div class="content">
            <h1>Mars</h1>
            <p>Mars is the fourth planet from the Sun, a cold, rocky "Red Planet" half the size of Earth with a thin, carbon-dioxide-dominated atmosphere. It has a 24.6-hour day, two tiny moons (Phobos and Deimos), and features extreme geography like Olympus Mons, the solar system’s tallest volcano. Evidence suggests a past with water, with current research focusing on exploring its surface for signs of ancient life.</p>
            <div class="facts">
                <div class="fact">Olympus Mons: Largest volcano</div>
                <div class="fact">Moons: Phobos & Deimos</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Mars" target="_blank" rel="noopener noreferrer">Mars - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Jupiter -->
    <section class="space-section" id="jupiter" style="background-image: url('https://storage.noirlab.edu/media/archives/images/screen/noirlab2116e.jpg');">
        <div class="content">
            <h1>Jupiter</h1>
            <p>Jupiter is the largest planet in our solar system, classified as a gas giant with no solid surface and primarily composed of hydrogen and helium. Located 5.2 AU from the Sun, it has a rapid 10-hour rotation, a massive, stormy atmosphere featuring the Great Red Spot, and 95 official moons, including volcanic Io and icy Europa.</p>
            <div class="facts">
                <div class="fact">Type: Gas giant</div>
                <div class="fact">Moons: 79+ (including Ganymede)</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Jupiter" target="_blank" rel="noopener noreferrer">Jupiter - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Saturn -->
    <section class="space-section" id="saturn" style="background-image: url('https://jatan.space/content/images/size/w1200/2024/02/562fd565-dfcd-480b-839e-561372ed9ca0_1024x577-jpeg.jpg');">
        <div class="content">
            <h1>Saturn</h1>
            <p>Saturn is the sixth planet from the Sun and the second-largest in the solar system, best known for its extensive, bright ring system. A gas giant primarily composed of hydrogen and helium, Saturn is less dense than water, has 274 known moons, including the methane-rich Titan, and experiences rapid 10.7-hour days.</p>
            <div class="facts">
                <div class="fact">Rings: Prominent</div>
                <div class="fact">Major moon: Titan</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Saturn" target="_blank" rel="noopener noreferrer">Saturn - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Uranus -->
    <section class="space-section" id="uranus" style="background-image: url('https://ychef.files.bbci.co.uk/1280x720/p0257vk5.jpg');">
        <div class="content">
            <h1>Uranus</h1>
            <p>.Uranus is the seventh planet from the Sun, classified as an icy giant with a distinct blue-green color due to methane in its hydrogen-helium atmosphere. It is unique for rotating on its side—with an axial tilt of 98°—and having 13 faint rings and 28 known moons. Discovered in 1781 by William Herschel, it is the coldest planet in the solar system.</p>
            <div class="facts">
                <div class="fact">Axis tilt: ~98°</div>
                <div class="fact">Color: Blue-green (methane)</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Uranus" target="_blank" rel="noopener noreferrer">Uranus - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Neptune -->
    <section class="space-section" id="neptune" style="background-image: url('https://science.nasa.gov/wp-content/uploads/2023/06/neptune-pia01492-1920x640-2-jpg.webp?w=1536');">
        <div class="content">
            <h1>Neptune</h1>
            <p>Neptune is the eighth and most distant planet from the Sun, known as a cold, dark ice giant with supersonic winds reaching Composed mainly of water, ammonia, and methane over a solid core, its methane-rich atmosphere gives it a vivid blue color. It has 14 known moons—notably the geyser-active Triton—and a faint ring system.</p>
            <div class="facts">
                <div class="fact">Wind speeds: Up to 2,000 km/h</div>
                <div class="fact">Great Dark Spot: Storm</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Neptune" target="_blank" rel="noopener noreferrer">Neptune - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Dwarf planets & Kuiper belt -->
    <section class="space-section" id="dwarf" style="background-image: url('https://science.nasa.gov/wp-content/uploads/2023/07/dwarf-planets.jpg');">
        <div class="content">
            <h1>Dwarf Planets & Kuiper Belt</h1>
            <p>The Kuiper Belt is a vast, doughnut-shaped region of icy bodies beyond Neptune (30–55 AU), containing thousands of objects (KBOs) left over from the solar system's formation 4.5 billion years ago. It hosts several dwarf planets—Pluto, Haumea, Makemake, and Eris (scattered disc)—and is the source of many short-period comets.</p>
            <div class="facts">
                <div class="fact">Pluto: Dwarf planet</div>
                <div class="fact">Kuiper Belt: Icy objects</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Kuiper_belt" target="_blank" rel="noopener noreferrer">Kuiper Belt - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Popular stars -->
    <section class="space-section" id="stars" style="background-image: url('https://preview.redd.it/these-are-the-16-brightest-stars-in-night-sky-v0-bopp27bgpkr31.jpg?width=640&crop=smart&auto=webp&s=dbf1af2276c07deb043b14d7e828ff1f144abf9a');">
        <div class="content">
            <h1>Stars</h1>
            <p>Stars: Facts about stellar formation Stars are massive, luminous spheres of plasma—primarily hydrogen and helium—held together by their own gravity. They produce intense heat and light through nuclear fusion in their cores. Born within nebulae, stars have life cycles lasting millions to billions of years, with colors indicating temperature (blue being hottest, red coolest). </p>

            <div class="star-grid">
                <div class="star-card">
                    <strong>Sirius</strong> <span class="muted">— The brightest star in the night sky; part of Canis Major</span>
                    <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Sirius" target="_blank" rel="noopener noreferrer">Sirius - Wikipedia</a></div>
                </div>
                <div class="star-card">
                    <strong>Betelgeuse</strong> <span class="muted">— A red supergiant in Orion, variable and nearing the end of life</span>
                    <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Betelgeuse" target="_blank" rel="noopener noreferrer">Betelgeuse - Wikipedia</a></div>
                </div>
                <div class="star-card">
                    <strong>Polaris</strong> <span class="muted">— The North Star, near the north celestial pole</span>
                    <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Polaris" target="_blank" rel="noopener noreferrer">Polaris - Wikipedia</a></div>
                </div>
                <div class="star-card">
                    <strong>Vega</strong> <span class="muted">— Bright star in Lyra, reference for photometric systems</span>
                    <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Vega" target="_blank" rel="noopener noreferrer">Vega - Wikipedia</a></div>
                </div>
                <div class="star-card">
                    <strong>Rigel</strong> <span class="muted">— A blue supergiant in Orion</span>
                    <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Rigel" target="_blank" rel="noopener noreferrer">Rigel - Wikipedia</a></div>
                </div>
            </div>
        </div>
    </section>

    <!-- Milky Way -->
    <section class="space-section" id="milkyway" style="background-image: url('https://media.springernature.com/full/springer-static/image/art%3A10.1038%2F490024a/MediaObjects/41586_2012_Article_BF490024a_Figc_HTML.jpg');">
        <div class="content">
            <h1>Milky Way Galaxy</h1>
            <p>The Milky Way is a barred spiral galaxy containing 100–400 billion stars, including our Sun, and spanning roughly 100,000–120,000 light-years in diameter. Located within the Orion Arm, our solar system sits about 26,000–27,000 light-years from the galactic center, which hosts a supermassive black hole.</p>
            <div class="facts">
                <div class="fact">Type: Barred spiral</div>
                <div class="fact">Diameter: ~100,000 light-years</div>
                              <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Milky_Way" target="_blank" rel="noopener noreferrer">Milky Way - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Blackhole -->
    <section class="space-section" id="blackhole" style="background-image: url('https://scitechdaily.com/images/Black-Hole-Visualization.jpg');">
        <div class="content">
            <h1>Black hole</h1>
            <p>A black hole is a region in space with gravitational pull so strong that nothing, not even light, can escape, usually formed by the core collapse of a massive star. They are characterized by extreme density, a boundary called the event horizon, and a central singularity, appearing invisible but detected by their effects on surrounding matter. </p>
            <div class="facts">
                <div class="fact">A black hole could fit in your pocket</div>
                <div class="fact">There are likely millions of black holes in our galaxy, and we will probably never know where they are</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Black_hole" target="_blank" rel="noopener noreferrer">Black Hole - Wikipedia</a></div>
            </div>
        </div>
    </section>

    <!-- Andromeda & Others -->
    <section class="space-section" id="galaxies" style="background-image: url('https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?auto=format&fit=crop&w=1920&q=80');">
        <div class="content">
            <h1>Andromeda & Other Galaxies</h1>
            <p>The Andromeda Galaxy (M31) is the nearest large spiral galaxy to our Milky Way, located approximately 2.5 million light-years away. It is the largest member of the Local Group, spanning about 220,000 light-years and containing roughly 1 trillion stars, appearing as a hazy "nebulous smear" to the naked eye.</p>
            <div class="facts">
                <div class="fact">Andromeda: ~2.5 million ly away</div>
                <div class="fact">Observable Universe: ~2 trillion galaxies</div>
                <div class="fact">find more here: <a href="https://en.wikipedia.org/wiki/Andromeda_Galaxy" target="_blank" rel="noopener noreferrer">Andromeda Galaxy - Wikipedia</a></div>
            </div>
            <a class="btn" href="#sun" onclick="restart(); return false;">Return to Sun</a>
        </div>
    </section>

    <script>
        // Simple navigation dots based on sections
        const sections = Array.from(document.querySelectorAll('.space-section'));
        const dotsContainer = document.getElementById('navDots');

        sections.forEach((sec, idx) => {
            const dot = document.createElement('div');
            dot.className = 'dot';
            dot.title = sec.querySelector('h1')?.innerText || ('Section ' + (idx+1));
            dot.addEventListener('click', () => sec.scrollIntoView({behavior:'smooth'}));
            dotsContainer.appendChild(dot);
        });

        const dots = Array.from(document.querySelectorAll('.dot'));

        function updateActiveDot() {
            const fromTop = window.scrollY + window.innerHeight/2;
            sections.forEach((sec, i) => {
                const rect = sec.getBoundingClientRect();
                const top = window.scrollY + rect.top;
                const bottom = top + rect.height;
                if (fromTop >= top && fromTop < bottom) {
                    dots.forEach(d => d.classList.remove('active'));
                    dots[i].classList.add('active');
                }
            });
        }

        // Adjust overlay darkness slightly by depth
        function updateOverlayByScroll() {
            const maxScroll = document.body.scrollHeight - window.innerHeight;
            const pos = Math.max(0, Math.min(window.scrollY, maxScroll));
            // compute alpha from 0.45 (near) to 0.7 (far)
            const alpha = 0.45 + 0.25 * (pos / (maxScroll || 1));
            document.documentElement.style.setProperty('--overlay-alpha', alpha.toFixed(3));
        }

        // Track cursor position for comets
        let mouseX = 0, mouseY = 0;
        let cometX = [50, 100, 200], cometY = [50, 100, 200];
        const comets = document.querySelectorAll('.comet');
        const speed = 0.1; // Smoothing factor (0-1, lower = smoother)
        let frameCount = 0;

        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
        });

        function createTrail(x, y) {
            const trail = document.createElement('div');
            trail.className = 'comet-trail';
            const size = Math.random() * 8 + 4; // 4-12px
            trail.style.width = size + 'px';
            trail.style.height = size + 'px';
            trail.style.left = (x - size / 2) + 'px';
            trail.style.top = (y - size / 2) + 'px';
            trail.style.opacity = '1';
            document.body.appendChild(trail);

            // Fade out and remove
            const duration = 800; // milliseconds
            const startTime = Date.now();
            const fadeInterval = setInterval(() => {
                const elapsed = Date.now() - startTime;
                const progress = elapsed / duration;
                trail.style.opacity = Math.max(0, 1 - progress);
                if (progress >= 1) {
                    clearInterval(fadeInterval);
                    trail.remove();
                }
            }, 30);
        }

        function updateCometPosition() {
            comets.forEach((comet, idx) => {
                // Smooth follow with easing
                cometX[idx] += (mouseX - cometX[idx]) * speed;
                cometY[idx] += (mouseY - cometY[idx]) * speed;
                
                comet.style.left = (cometX[idx] - 15) + 'px'; // -15 to center
                comet.style.top = (cometY[idx] - 15) + 'px';

                // Create trail every 4 frames
                if (frameCount % 4 === idx) {
                    createTrail(cometX[idx], cometY[idx]);
                }
            });
            frameCount++;
            requestAnimationFrame(updateCometPosition);
        }

        window.addEventListener('scroll', () => {
            updateActiveDot();
            updateOverlayByScroll();
        }, { passive: true });

        // init
        updateActiveDot();
        updateOverlayByScroll();
        updateCometPosition();

        // Return-to-top function used by buttons
        function restart() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Expose restart for inline onclick
        window.restart = restart;
    </script>
</body>
</html>
