/* Druva portfolio interactions. Local certificate images/PDFs open in a preview overlay. */
const CONFIG = {
  resumeUrl: "https://druvakumarchatala.lovable.app/__l5e/assets-v1/2d36ebd6-563f-4fac-b093-5b9ec19ff965/druva-kumar-resume.pdf",
  projects: {
    hospital: {
      title: "Hospital Healthcare Analytics & Operations Dashboard",
      eyebrow: "HEALTHCARE ANALYTICS · 2026",
      description: "A healthcare analytics project exploring admissions, department performance, bed utilization, operational KPIs, and service trends.",
      tools: ["Excel", "SQL", "Python", "Power BI"]
    },
    ecommerce: {
      title: "Brazilian E-Commerce Sales Analysis Dashboard",
      eyebrow: "E-COMMERCE ANALYTICS · 2026",
      description: "An analysis of Brazilian e-commerce data focused on customer behavior, sales trends, order outcomes, and delivery performance.",
      tools: ["Power BI", "SQL", "Excel", "DAX"]
    },
    netflix: {
      title: "Netflix Content Analytics Dashboard",
      eyebrow: "CONTENT ANALYTICS · COMPLETED",
      description: "An interactive Netflix content analytics dashboard exploring catalogue distribution, movies and TV shows, genres, countries, and release trends.",
      tools: ["Power BI", "Data Visualization", "Dashboard Development", "Analytical Reporting"]
    },
    agriculture: {
      title: "Agriculture Analytics — Seasonal Performance Analysis",
      eyebrow: "AGRICULTURE ANALYTICS · COMPLETED",
      description: "Analyzes agricultural performance across farms to understand crop selection, irrigation methods, yield, revenue, and profitability.",
      tools: ["Python", "Pandas", "NumPy", "Matplotlib", "Google Colab"]
    },
    startup: {
      title: "India Startup Investment Intelligence",
      eyebrow: "INVESTMENT ANALYTICS · COMPLETED",
      description: "Explores Indian startup funding patterns, investment amounts, funding rounds, industry segments, city-level activity, and yearly trends.",
      tools: ["Python", "Pandas", "SQL / MySQL", "Excel"]
    }
  }
};

const $ = (selector, scope = document) => scope.querySelector(selector);
const $$ = (selector, scope = document) => [...scope.querySelectorAll(selector)];

const year = $("#year");
if (year) year.textContent = new Date().getFullYear();

// Scroll reveal, with a fallback for browsers without IntersectionObserver.
if ("IntersectionObserver" in window) {
  const revealObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add("is-visible");
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });
  $$(".reveal").forEach(el => revealObserver.observe(el));

  const navLinks = $$(".nav-link[href^='#']");
  const sectionObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        navLinks.forEach(link => link.classList.toggle("active", link.getAttribute("href") === `#${entry.target.id}`));
      }
    });
  }, { rootMargin: "-35% 0px -55% 0px" });
  $$("main section[id]").forEach(section => sectionObserver.observe(section));
} else {
  $$(".reveal").forEach(el => el.classList.add("is-visible"));
}

// Close mobile navigation after a section is selected.
$$(".navbar-nav .nav-link").forEach(link => {
  link.addEventListener("click", () => {
    const nav = $("#mainNav");
    if (nav && nav.classList.contains("show") && window.bootstrap) {
      window.bootstrap.Collapse.getOrCreateInstance(nav).hide();
    }
  });
});

// Project details modal.
const projectModal = $("#projectModal");
let previousProjectFocus = null;
function openProject(key) {
  const project = CONFIG.projects[key];
  if (!project || !projectModal) return;
  previousProjectFocus = document.activeElement;
  $("#modalEyebrow").textContent = project.eyebrow;
  $("#modalTitle").textContent = project.title;
  $("#modalDescription").textContent = project.description;
  $("#modalTools").replaceChildren(...project.tools.map(tool => {
    const span = document.createElement("span");
    span.textContent = tool;
    return span;
  }));
  projectModal.classList.add("is-open");
  projectModal.setAttribute("aria-hidden", "false");
  document.body.classList.add("modal-open");
  $(".modal-close", projectModal)?.focus();
}
function closeProject() {
  if (!projectModal) return;
  projectModal.classList.remove("is-open");
  projectModal.setAttribute("aria-hidden", "true");
  document.body.classList.remove("modal-open");
  previousProjectFocus?.focus();
}
$$("[data-project]").forEach(button => button.addEventListener("click", () => openProject(button.dataset.project)));
$$("[data-close-modal]").forEach(button => button.addEventListener("click", closeProject));

// Certificate viewer: supports local JPG/PNG/WebP images and PDF files. No public URL is needed.
const certificateViewer = $("#certificateViewer");
const certificateViewerContent = $("#certificateViewerContent");
let previousCertificateFocus = null;
function openCertificate(file, title = "Certificate preview") {
  if (!certificateViewer || !certificateViewerContent || !file) return;
  previousCertificateFocus = document.activeElement;
  certificateViewerContent.replaceChildren();
  const extension = file.split("?")[0].split(".").pop().toLowerCase();
  if (extension === "pdf") {
    const frame = document.createElement("iframe");
    frame.src = file;
    frame.title = title;
    frame.className = "certificate-pdf-frame";
    certificateViewerContent.append(frame);
  } else {
    const image = document.createElement("img");
    image.src = file;
    image.alt = title;
    image.className = "certificate-large-image";
    image.addEventListener("error", () => {
      certificateViewerContent.replaceChildren();
      const message = document.createElement("p");
      message.className = "certificate-viewer-error";
      message.textContent = `Could not find ${file}. Check that the filename and extension match the file in your assets folder.`;
      certificateViewerContent.append(message);
    }, { once: true });
    certificateViewerContent.append(image);
  }
  certificateViewer.classList.add("is-open");
  certificateViewer.setAttribute("aria-hidden", "false");
  document.body.classList.add("modal-open");
  $("[data-close-certificate]", certificateViewer)?.focus();
}
function closeCertificate() {
  if (!certificateViewer) return;
  certificateViewer.classList.remove("is-open");
  certificateViewer.setAttribute("aria-hidden", "true");
  certificateViewerContent?.replaceChildren();
  document.body.classList.remove("modal-open");
  previousCertificateFocus?.focus();
}
$$("[data-certificate-file]").forEach(link => {
  link.addEventListener("click", event => {
    event.preventDefault();
    openCertificate(link.dataset.certificateFile || link.getAttribute("href"), link.querySelector("img")?.alt || link.textContent.trim() || "Certificate preview");
  });
});
$$("[data-close-certificate]").forEach(button => button.addEventListener("click", closeCertificate));

// Résumé button: opens the configured PDF in a new tab.
const resumeButton = $("#resumeDownload");
if (resumeButton) {
  resumeButton.addEventListener("click", event => {
    if (!CONFIG.resumeUrl) {
      event.preventDefault();
      showToast("Add your résumé PDF path to CONFIG.resumeUrl in js/script.js.");
    }
  });
}

function showToast(message) {
  const toast = $("#toastMessage");
  if (!toast) return;
  toast.textContent = message;
  toast.classList.add("show");
  window.clearTimeout(showToast.timer);
  showToast.timer = window.setTimeout(() => toast.classList.remove("show"), 3300);
}

document.addEventListener("keydown", event => {
  if (event.key === "Escape") {
    if (certificateViewer?.classList.contains("is-open")) closeCertificate();
    else if (projectModal?.classList.contains("is-open")) closeProject();
  }
});

// Subtle header treatment after scrolling.
const header = $(".site-header");
if (header) {
  const updateHeader = () => {
    header.style.background = window.scrollY > 20 ? "rgba(8,8,8,.96)" : "rgba(8,8,8,.82)";
  };
  window.addEventListener("scroll", updateHeader, { passive: true });
  updateHeader();
}
