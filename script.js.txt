/* basic interactions for MatCen landing page */

// tampilkan tahun sekarang
const yearEl = document.getElementById('year');
if (yearEl) yearEl.textContent = new Date().getFullYear();

// mobile menu toggle
const menuBtn = document.getElementById('menuBtn');
const mobileMenu = document.getElementById('mobileMenu');
if (menuBtn && mobileMenu) {
  menuBtn.addEventListener('click', () => {
    const open = menuBtn.getAttribute('aria-expanded') === 'true';
    menuBtn.setAttribute('aria-expanded', String(!open));
    if (open) {
      mobileMenu.hidden = true;
    } else {
      mobileMenu.hidden = false;
    }
  });
}

// sample categories data (bisa diganti / ambil dari API nantinya)
const categoriesData = [
  { id: 'aluminium', title: 'Aluminium', group: 'metal', desc: 'Lembaran, batang, dan bahan olahan aluminium.' },
  { id: 'kuningan', title: 'Kuningan', group: 'metal', desc: 'Bahan kuningan untuk komponen mesin & elektrik.' },
  { id: 'kapas', title: 'Kapas', group: 'textile', desc: 'Serat kapas untuk kebutuhan tekstil.' },
  { id: 'karet', title: 'Karet', group: 'polymer', desc: 'Karet alam & olahan untuk industri.' },
  { id: 'plastik', title: 'Plastik', group: 'polymer', desc: 'Granul plastik dan bahan baku polimer.' },
  { id: 'besi', title: 'Besi & Baja', group: 'metal', desc: 'Plat, batang, serta besi cor.' }
];

// render categories grid
const grid = document.getElementById('categoriesGrid');
function renderCategories(list) {
  if (!grid) return;
  grid.innerHTML = '';
  if (list.length === 0) {
    grid.innerHTML = '<p class="muted">Tidak ada kategori yang cocok.</p>';
    return;
  }
  list.forEach(cat => {
    const el = document.createElement('div');
    el.className = 'category-card';
    el.innerHTML = `
      <h4>${escapeHtml(cat.title)}</h4>
      <p class="category-meta">${escapeHtml(cat.desc)}</p>
      <div style="margin-top:.6rem">
        <a class="link" href="#contact" data-cat="${cat.id}">Lihat Supplier →</a>
      </div>
    `;
    grid.appendChild(el);
  });
}

// escape simple
function escapeHtml(s) {
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

// initial render
renderCategories(categoriesData);

// search + filter
const searchInput = document.getElementById('searchInput');
const filterSelect = document.getElementById('filterSelect');

function applyFilters() {
  const q = searchInput ? searchInput.value.trim().toLowerCase() : '';
  const f = filterSelect ? filterSelect.value : 'all';
  const filtered = categoriesData.filter(c => {
    const matchQ = q === '' || c.title.toLowerCase().includes(q) || c.desc.toLowerCase().includes(q);
    const matchF = f === 'all' || c.group === f;
    return matchQ && matchF;
  });
  renderCategories(filtered);
}

if (searchInput) searchInput.addEventListener('input', applyFilters);
if (filterSelect) filterSelect.addEventListener('change', applyFilters);

// contact form simple validation & fake send
const contactForm = document.getElementById('contactForm');
const formStatus = document.getElementById('formStatus');
if (contactForm) {
  contactForm.addEventListener('submit', (e) => {
    e.preventDefault();
    const name = document.getElementById('name').value.trim();
    const email = document.getElementById('email').value.trim();
    const msg = document.getElementById('message').value.trim();
    if (!name || !email || !msg) {
      if (formStatus) formStatus.textContent = 'Mohon lengkapi semua kolom.';
      return;
    }
    // simulasi pengiriman
    if (formStatus) formStatus.textContent = 'Mengirim pesan...';
    setTimeout(() => {
      if (formStatus) formStatus.textContent = 'Pesan terkirim. Tim MatCen akan menghubungi Anda.';
      contactForm.reset();
    }, 900);
  });

  const resetBtn = document.getElementById('resetBtn');
  if (resetBtn) resetBtn.addEventListener('click', () => {
    contactForm.reset();
    if (formStatus) formStatus.textContent = '';
  });
}
