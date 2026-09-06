
(function () {
  "use strict";

  const topbar = document.querySelector(".topbar");
  const onScroll = () => {
    if (!topbar) return;
    topbar.classList.toggle("scrolled", window.scrollY > 40);
  };
  window.addEventListener("scroll", onScroll, { passive: true });
  onScroll();

  const here = (location.pathname.split("/").pop() || "index.html").toLowerCase();
  document.querySelectorAll('.nav a[href]').forEach((a) => {
    const h = (a.getAttribute("href") || "").toLowerCase();
    if (h === here) a.classList.add("is-active");
  });

  const ddItems = document.querySelectorAll(".nav li.has-drop");
  ddItems.forEach((li) => {
    const btn = li.querySelector("button.nav-link");
    if (!btn) return;
    btn.addEventListener("click", (e) => {
      e.stopPropagation();
      const open = li.classList.contains("open");
      ddItems.forEach((x) => x.classList.remove("open"));
      li.classList.toggle("open", !open);
      btn.setAttribute("aria-expanded", String(!open));
    });
  });
  document.addEventListener("click", () => ddItems.forEach((x) => x.classList.remove("open")));

  const burger = document.querySelector(".burger");
  const nav = document.querySelector(".nav");
  if (burger && nav) {
    burger.addEventListener("click", (e) => {
      e.stopPropagation();
      const open = nav.classList.toggle("open");
      burger.setAttribute("aria-expanded", String(open));
      document.body.style.overflow = open ? "hidden" : "";
    });
    nav.addEventListener("click", (e) => {
      if (e.target.closest("a") && !e.target.closest(".drop a")) {
        nav.classList.remove("open");
        burger.setAttribute("aria-expanded", "false");
        document.body.style.overflow = "";
      }
    });
  }

  const revs = document.querySelectorAll(".reveal");
  if ("IntersectionObserver" in window && revs.length) {
    const io = new IntersectionObserver(
      (es) => es.forEach((en) => { if (en.isIntersecting) { en.target.classList.add("in"); io.unobserve(en.target); } }),
      { threshold: 0.08, rootMargin: "0px 0px -6% 0px" }
    );
    revs.forEach((el) => io.observe(el));
  } else revs.forEach((el) => el.classList.add("in"));

  document.querySelectorAll("[data-count]").forEach((el) => {
    const target = parseFloat(el.dataset.count);
    if (!isFinite(target)) return;
    const suf = el.dataset.suffix || "";
    const dec = el.dataset.decimals | 0;
    const draw = (v) => (el.textContent = v.toFixed(dec).replace(/\B(?=(\d{3})+(?!\d))/g, ",") + suf);
    if (!("IntersectionObserver" in window)) return draw(target);
    const io = new IntersectionObserver((es) => {
      if (!es[0].isIntersecting) return;
      io.disconnect();
      const t0 = performance.now(), dur = 1400;
      const tick = (t) => {
        const p = Math.min(1, (t - t0) / dur);
        draw(target * (1 - Math.pow(1 - p, 3)));
        if (p < 1) requestAnimationFrame(tick);
      };
      requestAnimationFrame(tick);
    }, { threshold: 0.5 });
    io.observe(el);
  });

  const slides = document.querySelectorAll(".hero-slides img");
  if (slides.length > 1) {
    let i = 0;
    slides[0].classList.add("on");
    setInterval(() => {
      slides[i].classList.remove("on");
      i = (i + 1) % slides.length;
      slides[i].classList.add("on");
    }, 6000);
  } else if (slides.length === 1) slides[0].classList.add("on");

  const quotes = document.querySelectorAll(".quote-slide");
  if (quotes.length) {
    const dotsWrap = document.querySelector(".quote-dots");
    let qi = 0, timer = null;
    quotes.forEach((_, n) => {
      if (!dotsWrap) return;
      const d = document.createElement("i");
      d.addEventListener("click", () => show(n));
      dotsWrap.appendChild(d);
    });
    const dots = dotsWrap ? dotsWrap.children : [];
    function show(n) {
      quotes[qi].style.display = "none";
      qi = (n + quotes.length) % quotes.length;
      quotes[qi].style.display = "block";
      quotes[qi].style.animation = "none";
      void quotes[qi].offsetWidth;
      quotes[qi].style.animation = "rise 0.7s cubic-bezier(0.22,0.61,0.36,1)";
      Array.from(dots).forEach((d, k) => d.classList.toggle("on", k === qi));
      restart();
    }
    function restart() { clearInterval(timer); timer = setInterval(() => show(qi + 1), 7000); }
    const prev = document.querySelector("#quote-prev");
    const next = document.querySelector("#quote-next");
    if (prev) prev.addEventListener("click", () => show(qi - 1));
    if (next) next.addEventListener("click", () => show(qi + 1));
    show(0);
  }

  document.querySelectorAll("[data-rail]").forEach((rail) => {
    const prev = document.querySelector(rail.dataset.railPrev);
    const next = document.querySelector(rail.dataset.railNext);
    const step = () => (rail.querySelector(":scope > *")?.getBoundingClientRect().width || 340) + 22;
    if (next) next.addEventListener("click", () => rail.scrollBy({ left: step(), behavior: "smooth" }));
    if (prev) prev.addEventListener("click", () => rail.scrollBy({ left: -step(), behavior: "smooth" }));
  });

  const lb = document.getElementById("lightbox");
  if (lb) {
    const img = lb.querySelector("img");
    const cap = lb.querySelector(".cap");
    const shots = Array.from(document.querySelectorAll(".gallery button"));
    let li = 0;
    function open(i) {
      li = i;
      const b = shots[i];
      img.src = b.querySelector("img").src;
      img.alt = b.dataset.cap || "";
      cap.textContent = b.dataset.cap || "";
      lb.classList.add("on");
      document.body.style.overflow = "hidden";
    }
    function close() { lb.classList.remove("on"); document.body.style.overflow = ""; }
    shots.forEach((b, i) => b.addEventListener("click", () => open(i)));
    lb.querySelector(".x").addEventListener("click", close);
    lb.querySelector(".pv").addEventListener("click", () => open((li - 1 + shots.length) % shots.length));
    lb.querySelector(".nx").addEventListener("click", () => open((li + 1) % shots.length));
    lb.addEventListener("click", (e) => { if (e.target === lb) close(); });
    document.addEventListener("keydown", (e) => {
      if (!lb.classList.contains("on")) return;
      if (e.key === "Escape") close();
      if (e.key === "ArrowLeft") open((li - 1 + shots.length) % shots.length);
      if (e.key === "ArrowRight") open((li + 1) % shots.length);
    });
  }

  const chipWrap = document.getElementById("filter-chips");
  if (chipWrap) {
    const chips = Array.from(chipWrap.querySelectorAll(".chip"));
    const items = Array.from(document.querySelectorAll("[data-type]"));
    chips.forEach((c) => c.addEventListener("click", () => {
      chips.forEach((x) => x.classList.remove("is-on"));
      c.classList.add("is-on");
      const t = c.dataset.filter;
      items.forEach((it) => {
        const show = t === "all" || (it.dataset.type || "").split(" ").includes(t);
        it.classList.toggle("hidden", !show);
      });
    }));
  }

  document.querySelectorAll("form[data-fake]").forEach((f) => {
    f.addEventListener("submit", (e) => {
      e.preventDefault();
      if (!f.reportValidity()) return;
      const ok = f.parentElement.querySelector(".form-ok") || f.querySelector(".form-ok");
      if (ok) ok.classList.add("show");
      f.reset();
      setTimeout(() => ok && ok.classList.remove("show"), 7000);
    });
  });

  document.querySelectorAll("[data-year]").forEach((n) => (n.textContent = new Date().getFullYear()));

  document.querySelectorAll('a[href^="#"]').forEach((a) => {
    a.addEventListener("click", (e) => {
      const id = a.getAttribute("href").slice(1);
      if (!id) return;
      const t = document.getElementById(id);
      if (!t) return;
      e.preventDefault();
      window.scrollTo({ top: t.getBoundingClientRect().top + window.scrollY - 88, behavior: "smooth" });
      history.replaceState(null, "", "#" + id);
    });
  });
})();

