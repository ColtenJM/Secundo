import React, { useState, useMemo, useRef } from "react";

// ---------- Seed data ----------
const SEED_LISTINGS = [
  { id: 1, brand: "Rolex", model: "Datejust 36", price: 8200, condition: "Excellent", year: 2018, city: "Edmonton, AB", distance: 2.1, lat: 28, lng: 32, photo: "linear-gradient(135deg,#2b2b2b,#5c5c5c)", seller: "M. Kowalski", rating: 4.9, papers: true, desc: "Box and papers, full link bracelet, recently serviced.",
    movement: "Automatic", caseSize: "36mm", caseMaterial: "Stainless Steel", strap: "Bracelet (Jubilee)", waterResist: "100m", crystal: "Sapphire", dialColor: "Silver" },
  { id: 2, brand: "Omega", model: "Speedmaster Pro", price: 5400, condition: "Very Good", year: 2015, city: "Edmonton, AB", distance: 4.7, lat: 40, lng: 55, photo: "linear-gradient(135deg,#1a1a1a,#3a3a3a)", seller: "D. Renner", rating: 4.7, papers: true, desc: "Moonwatch, hesalite crystal, original box.",
    movement: "Manual", caseSize: "42mm", caseMaterial: "Stainless Steel", strap: "Bracelet (Steel)", waterResist: "50m", crystal: "Hesalite", dialColor: "Black" },
  { id: 3, brand: "Seiko", model: "SKX007", price: 280, condition: "Good", year: 2012, city: "St. Albert, AB", distance: 11.3, lat: 60, lng: 20, photo: "linear-gradient(135deg,#0d2b3e,#1d4e6b)", seller: "J. Fontaine", rating: 4.5, papers: false, desc: "Classic diver, some wear on case, runs strong.",
    movement: "Automatic", caseSize: "42.5mm", caseMaterial: "Stainless Steel", strap: "Rubber", waterResist: "200m", crystal: "Hardlex", dialColor: "Black" },
  { id: 4, brand: "Tudor", model: "Black Bay 58", price: 3650, condition: "Excellent", year: 2021, city: "Sherwood Park, AB", distance: 16.8, lat: 75, lng: 65, photo: "linear-gradient(135deg,#1c1c1c,#3d3d3d)", seller: "A. Singh", rating: 5.0, papers: true, desc: "Full kit, barely worn, still has stickers.",
    movement: "Automatic", caseSize: "39mm", caseMaterial: "Stainless Steel", strap: "Bracelet (Steel)", waterResist: "200m", crystal: "Sapphire", dialColor: "Blue" },
  { id: 5, brand: "Casio", model: "G-Shock GA-2100", price: 75, condition: "Good", year: 2022, city: "Edmonton, AB", distance: 3.4, lat: 35, lng: 45, photo: "linear-gradient(135deg,#101010,#2a2a2a)", seller: "T. Nguyen", rating: 4.3, papers: false, desc: "CasiOak, light scratches on bezel, works perfectly.",
    movement: "Quartz", caseSize: "45mm", caseMaterial: "Resin", strap: "Resin", waterResist: "200m", crystal: "Mineral", dialColor: "Black" },
  { id: 6, brand: "Tag Heuer", model: "Carrera", price: 1950, condition: "Very Good", year: 2017, city: "Edmonton, AB", distance: 6.2, lat: 48, lng: 30, photo: "linear-gradient(135deg,#27211a,#534838)", seller: "R. Okafor", rating: 4.8, papers: true, desc: "Chronograph, leather strap, recently battery serviced.",
    movement: "Quartz", caseSize: "41mm", caseMaterial: "Stainless Steel", strap: "Leather", waterResist: "100m", crystal: "Sapphire", dialColor: "Black" },
  { id: 7, brand: "Citizen", model: "Eco-Drive Promaster", price: 220, condition: "Excellent", year: 2020, city: "Leduc, AB", distance: 23.5, lat: 85, lng: 75, photo: "linear-gradient(135deg,#0c2a2e,#1f5258)", seller: "K. Bjornstad", rating: 4.6, papers: false, desc: "Solar powered, never needs a battery, mint condition.",
    movement: "Solar", caseSize: "44mm", caseMaterial: "Stainless Steel", strap: "Rubber", waterResist: "200m", crystal: "Mineral", dialColor: "Black" },
  { id: 8, brand: "Hamilton", model: "Khaki Field", price: 410, condition: "Excellent", year: 2023, city: "Edmonton, AB", distance: 1.5, lat: 25, lng: 50, photo: "linear-gradient(135deg,#2e2418,#5a4830)", seller: "P. Dubois", rating: 4.9, papers: true, desc: "Hand-wound, canvas strap, like new.",
    movement: "Manual", caseSize: "38mm", caseMaterial: "Stainless Steel", strap: "Canvas", waterResist: "50m", crystal: "Sapphire", dialColor: "Black" },
];

const BRANDS = ["All brands", ...Array.from(new Set(SEED_LISTINGS.map(l => l.brand)))];
const MOVEMENTS = ["Any movement", "Automatic", "Manual", "Quartz", "Solar"];
const MATERIALS = ["Any material", ...Array.from(new Set(SEED_LISTINGS.map(l => l.caseMaterial)))];
const STRAPS = ["Any strap", ...Array.from(new Set(SEED_LISTINGS.map(l => l.strap)))];
const CRYSTALS = ["Any crystal", ...Array.from(new Set(SEED_LISTINGS.map(l => l.crystal)))];
const CONDITIONS = ["Any condition", "Excellent", "Very Good", "Good", "Fair"];

// ---------- Helpers ----------
function fmtPrice(p) {
  return "$" + p.toLocaleString();
}

function initials(name) {
  return name.split(" ").map(s => s[0]).join("").slice(0, 2).toUpperCase();
}

// ---------- App ----------
export default function Secundo() {
  const [query, setQuery] = useState("");
  const [model, setModel] = useState("");
  const [brand, setBrand] = useState("All brands");
  const [movement, setMovement] = useState("Any movement");
  const [material, setMaterial] = useState("Any material");
  const [strap, setStrap] = useState("Any strap");
  const [crystal, setCrystal] = useState("Any crystal");
  const [condition, setCondition] = useState("Any condition");
  const [maxPrice, setMaxPrice] = useState(10000);
  const [radius, setRadius] = useState(25);
  const [selected, setSelected] = useState(null);
  const [view, setView] = useState("list"); // list | map
  const [showMessages, setShowMessages] = useState(false);
  const [showNewListing, setShowNewListing] = useState(false);
  const [threads, setThreads] = useState({}); // listingId -> [{from, text}]
  const [draft, setDraft] = useState("");
  const [myListings, setMyListings] = useState([]);
  const [toast, setToast] = useState(null);
  const toastTimer = useRef(null);

  const allListings = useMemo(() => [...myListings, ...SEED_LISTINGS], [myListings]);

  const filtered = useMemo(() => {
    return allListings.filter(l => {
      if (brand !== "All brands" && l.brand !== brand) return false;
      if (movement !== "Any movement" && l.movement !== movement) return false;
      if (material !== "Any material" && l.caseMaterial !== material) return false;
      if (strap !== "Any strap" && l.strap !== strap) return false;
      if (crystal !== "Any crystal" && l.crystal !== crystal) return false;
      if (condition !== "Any condition" && l.condition !== condition) return false;
      if (l.price > maxPrice) return false;
      if (l.distance > radius) return false;
      if (model.trim() && !l.model.toLowerCase().includes(model.trim().toLowerCase())) return false;
      if (query.trim()) {
        const q = query.toLowerCase();
        const hay = [l.brand, l.model, l.movement, l.caseMaterial, l.strap, l.dialColor].join(" ").toLowerCase();
        if (!hay.includes(q)) return false;
      }
      return true;
    }).sort((a, b) => a.distance - b.distance);
  }, [allListings, brand, movement, material, strap, crystal, condition, maxPrice, radius, query, model]);

  function resetFilters() {
    setQuery(""); setModel(""); setBrand("All brands"); setMovement("Any movement");
    setMaterial("Any material"); setStrap("Any strap"); setCrystal("Any crystal");
    setCondition("Any condition"); setMaxPrice(10000); setRadius(25);
  }

  function showToast(msg) {
    setToast(msg);
    clearTimeout(toastTimer.current);
    toastTimer.current = setTimeout(() => setToast(null), 2400);
  }

  function sendMessage(listingId) {
    if (!draft.trim()) return;
    setThreads(t => {
      const existing = t[listingId] || [];
      return {
        ...t,
        [listingId]: [
          ...existing,
          { from: "you", text: draft },
          { from: "seller", text: "Thanks for reaching out — happy to meet up. Does this weekend work for you?" },
        ],
      };
    });
    setDraft("");
  }

  function openListing(l) {
    setSelected(l);
  }

  function handleCreateListing(newListing) {
    setMyListings(m => [{ ...newListing, id: Date.now() }, ...m]);
    setShowNewListing(false);
    showToast("Listing published.");
  }

  return (
    <div style={styles.app}>
      <style>{fontImport}</style>

      {/* Header */}
      <header style={styles.header}>
        <div style={styles.headerInner}>
          <div style={styles.brandMark}>
            <span style={styles.brandDial} />
            <span style={styles.brandName}>Secundo</span>
          </div>
          <div style={styles.headerActions}>
            <button style={styles.ghostBtn} onClick={() => setShowMessages(true)}>
              Messages{Object.keys(threads).length > 0 ? ` (${Object.keys(threads).length})` : ""}
            </button>
            <button style={styles.primaryBtn} onClick={() => setShowNewListing(true)}>+ List a watch</button>
          </div>
        </div>
      </header>

      {/* Slim top bar: free-text search + view toggle */}
      <div style={styles.searchBar}>
        <div style={styles.searchBarInner}>
          <input
            style={styles.searchInput}
            placeholder="Search brand, model, movement, material..."
            value={query}
            onChange={e => setQuery(e.target.value)}
          />
          <div style={styles.viewToggle}>
            <button
              onClick={() => setView("list")}
              style={{ ...styles.toggleBtn, ...(view === "list" ? styles.toggleBtnActive : {}) }}
            >List</button>
            <button
              onClick={() => setView("map")}
              style={{ ...styles.toggleBtn, ...(view === "map" ? styles.toggleBtnActive : {}) }}
            >Map</button>
          </div>
        </div>
      </div>

      {/* Main: spec sidebar (left) + results */}
      <main style={styles.mainLayout} className="secundo-main-layout">
        <aside style={styles.sidebar} className="secundo-sidebar">
          <div style={styles.sidebarHeadRow}>
            <span style={styles.sidebarTitle}>Specifications</span>
            <button style={styles.resetLink} onClick={resetFilters}>Reset</button>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Brand</label>
            <select style={styles.filterSelect} value={brand} onChange={e => setBrand(e.target.value)}>
              {BRANDS.map(b => <option key={b} value={b}>{b}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Model</label>
            <input style={styles.filterInput} placeholder="e.g. Speedmaster" value={model} onChange={e => setModel(e.target.value)} />
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Movement</label>
            <select style={styles.filterSelect} value={movement} onChange={e => setMovement(e.target.value)}>
              {MOVEMENTS.map(m => <option key={m} value={m}>{m}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Case material</label>
            <select style={styles.filterSelect} value={material} onChange={e => setMaterial(e.target.value)}>
              {MATERIALS.map(m => <option key={m} value={m}>{m}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Strap</label>
            <select style={styles.filterSelect} value={strap} onChange={e => setStrap(e.target.value)}>
              {STRAPS.map(s => <option key={s} value={s}>{s}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Crystal</label>
            <select style={styles.filterSelect} value={crystal} onChange={e => setCrystal(e.target.value)}>
              {CRYSTALS.map(c => <option key={c} value={c}>{c}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Condition</label>
            <select style={styles.filterSelect} value={condition} onChange={e => setCondition(e.target.value)}>
              {CONDITIONS.map(c => <option key={c} value={c}>{c}</option>)}
            </select>
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Price — up to {fmtPrice(maxPrice)}</label>
            <input type="range" min="50" max="10000" step="50" value={maxPrice}
              onChange={e => setMaxPrice(Number(e.target.value))} style={styles.filterSlider} />
          </div>

          <div style={styles.filterGroup}>
            <label style={styles.filterLabel}>Distance — within {radius} mi</label>
            <input type="range" min="1" max="50" step="1" value={radius}
              onChange={e => setRadius(Number(e.target.value))} style={styles.filterSlider} />
          </div>
        </aside>

        <section style={styles.results}>
          <div style={styles.resultsHeader}>
            <span style={styles.resultsCount}>{filtered.length} watch{filtered.length !== 1 ? "es" : ""} nearby</span>
          </div>

          {view === "list" ? (
            <div style={styles.grid}>
              {filtered.map(l => (
                <div key={l.id} style={styles.card} onClick={() => openListing(l)}>
                  <div style={{ ...styles.cardPhoto, background: l.photo }}>
                    <span style={styles.cardDistance}>{l.distance} mi</span>
                  </div>
                  <div style={styles.cardBody}>
                    <div style={styles.cardTitleRow}>
                      <span style={styles.cardBrand}>{l.brand}</span>
                      <span style={styles.cardPrice}>{fmtPrice(l.price)}</span>
                    </div>
                    <div style={styles.cardModel}>{l.model}</div>
                    <div style={styles.cardMeta}>{l.condition} · {l.year} · {l.city}</div>
                    <div style={styles.cardSpecRow}>
                      <span style={styles.miniTag}>{l.movement}</span>
                      <span style={styles.miniTag}>{l.caseSize}</span>
                      <span style={styles.miniTag}>{l.caseMaterial}</span>
                    </div>
                  </div>
                </div>
              ))}
              {filtered.length === 0 && (
                <div style={styles.empty}>No watches match those filters yet. Try widening your radius or price range.</div>
              )}
            </div>
          ) : (
            <div style={styles.mapWrap}>
              <div style={styles.mapCanvas}>
                {filtered.map(l => (
                  <button
                    key={l.id}
                    onClick={() => openListing(l)}
                    style={{ ...styles.mapPin, left: `${l.lng}%`, top: `${l.lat}%` }}
                    title={`${l.brand} ${l.model}`}
                  >
                    <span style={styles.mapPinPrice}>{fmtPrice(l.price)}</span>
                  </button>
                ))}
                <div style={styles.mapCenter} />
                <span style={styles.mapNote}>Map preview — pins are approximate</span>
              </div>
            </div>
          )}
        </section>
      </main>

      {/* Listing detail modal */}
      {selected && (
        <div style={styles.overlay} onClick={() => setSelected(null)}>
          <div style={styles.modal} onClick={e => e.stopPropagation()}>
            <button style={styles.closeBtn} onClick={() => setSelected(null)}>×</button>
            <div style={{ ...styles.modalPhoto, background: selected.photo }} />
            <div style={styles.modalBody}>
              <div style={styles.modalTitleRow}>
                <div>
                  <div style={styles.modalBrand}>{selected.brand}</div>
                  <div style={styles.modalModel}>{selected.model}</div>
                </div>
                <div style={styles.modalPrice}>{fmtPrice(selected.price)}</div>
              </div>
              <div style={styles.tagRow}>
                <span style={styles.tag}>{selected.condition}</span>
                <span style={styles.tag}>{selected.year}</span>
                <span style={styles.tag}>{selected.distance} mi · {selected.city}</span>
                {selected.papers && <span style={styles.tagAccent}>Box & papers</span>}
              </div>
              <p style={styles.modalDesc}>{selected.desc}</p>

              <div style={styles.specGrid}>
                <div style={styles.specItem}><span style={styles.specLabel}>Movement</span><span style={styles.specValue}>{selected.movement}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Case size</span><span style={styles.specValue}>{selected.caseSize}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Case material</span><span style={styles.specValue}>{selected.caseMaterial}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Strap</span><span style={styles.specValue}>{selected.strap}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Water resistance</span><span style={styles.specValue}>{selected.waterResist}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Crystal</span><span style={styles.specValue}>{selected.crystal}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Dial color</span><span style={styles.specValue}>{selected.dialColor}</span></div>
                <div style={styles.specItem}><span style={styles.specLabel}>Year</span><span style={styles.specValue}>{selected.year}</span></div>
              </div>
              <div style={styles.sellerRow}>
                <div style={styles.avatar}>{initials(selected.seller)}</div>
                <div>
                  <div style={styles.sellerName}>{selected.seller}</div>
                  <div style={styles.sellerRating}>★ {selected.rating} local seller</div>
                </div>
              </div>

              <div style={styles.threadBox}>
                {(threads[selected.id] || []).map((m, i) => (
                  <div key={i} style={{ ...styles.bubble, ...(m.from === "you" ? styles.bubbleYou : styles.bubbleSeller) }}>
                    {m.text}
                  </div>
                ))}
                {!(threads[selected.id] && threads[selected.id].length) && (
                  <div style={styles.threadEmpty}>Message {selected.seller.split(" ")[0]} to ask questions or arrange a meetup.</div>
                )}
              </div>
              <div style={styles.composeRow}>
                <input
                  style={styles.composeInput}
                  placeholder="Is this still available?"
                  value={draft}
                  onChange={e => setDraft(e.target.value)}
                  onKeyDown={e => e.key === "Enter" && sendMessage(selected.id)}
                />
                <button style={styles.primaryBtn} onClick={() => sendMessage(selected.id)}>Send</button>
              </div>
            </div>
          </div>
        </div>
      )}

      {/* Messages drawer */}
      {showMessages && (
        <div style={styles.overlay} onClick={() => setShowMessages(false)}>
          <div style={styles.drawer} onClick={e => e.stopPropagation()}>
            <div style={styles.drawerHeader}>
              <span style={styles.drawerTitle}>Messages</span>
              <button style={styles.closeBtn} onClick={() => setShowMessages(false)}>×</button>
            </div>
            {Object.keys(threads).length === 0 && (
              <div style={styles.threadEmpty}>No conversations yet. Message a seller from any listing.</div>
            )}
            {Object.entries(threads).map(([id, msgs]) => {
              const l = allListings.find(x => x.id === Number(id));
              if (!l) return null;
              return (
                <div key={id} style={styles.threadRow} onClick={() => { setSelected(l); setShowMessages(false); }}>
                  <div style={{ ...styles.threadPhoto, background: l.photo }} />
                  <div>
                    <div style={styles.threadName}>{l.brand} {l.model}</div>
                    <div style={styles.threadLast}>{msgs[msgs.length - 1].text}</div>
                  </div>
                </div>
              );
            })}
          </div>
        </div>
      )}

      {/* New listing modal */}
      {showNewListing && (
        <NewListingForm
          onClose={() => setShowNewListing(false)}
          onSubmit={handleCreateListing}
        />
      )}

      {toast && <div style={styles.toast}>{toast}</div>}

      <footer style={styles.footer}>
        Secundo is a local marketplace for buying and selling watches in person. Always meet in a public place.
      </footer>
    </div>
  );
}

function NewListingForm({ onClose, onSubmit }) {
  const [form, setForm] = useState({
    brand: "", model: "", price: "", condition: "Very Good", year: "", city: "Edmonton, AB", papers: false, desc: "",
    movement: "Automatic", caseSize: "", caseMaterial: "Stainless Steel", strap: "Leather", waterResist: "", crystal: "Sapphire", dialColor: "",
  });
  const colors = ["linear-gradient(135deg,#2b2b2b,#5c5c5c)", "linear-gradient(135deg,#0d2b3e,#1d4e6b)", "linear-gradient(135deg,#27211a,#534838)", "linear-gradient(135deg,#1c1c1c,#3d3d3d)"];

  function set(k, v) { setForm(f => ({ ...f, [k]: v })); }

  function submit() {
    if (!form.brand.trim() || !form.model.trim() || !form.price) return;
    onSubmit({
      ...form,
      price: Number(form.price),
      year: Number(form.year) || new Date().getFullYear(),
      distance: 0.3,
      lat: 50, lng: 50,
      seller: "You",
      rating: 5.0,
      photo: colors[Math.floor(Math.random() * colors.length)],
    });
  }

  return (
    <div style={styles.overlay} onClick={onClose}>
      <div style={styles.modal} onClick={e => e.stopPropagation()}>
        <button style={styles.closeBtn} onClick={onClose}>×</button>
        <div style={styles.modalBody}>
          <div style={styles.formTitle}>List a watch</div>
          <div style={styles.formGrid}>
            <input style={styles.formInput} placeholder="Brand (e.g. Seiko)" value={form.brand} onChange={e => set("brand", e.target.value)} />
            <input style={styles.formInput} placeholder="Model (e.g. SKX007)" value={form.model} onChange={e => set("model", e.target.value)} />
            <input style={styles.formInput} placeholder="Price (USD)" type="number" value={form.price} onChange={e => set("price", e.target.value)} />
            <input style={styles.formInput} placeholder="Year" type="number" value={form.year} onChange={e => set("year", e.target.value)} />
            <select style={styles.formInput} value={form.condition} onChange={e => set("condition", e.target.value)}>
              {["Excellent", "Very Good", "Good", "Fair"].map(c => <option key={c}>{c}</option>)}
            </select>
            <input style={styles.formInput} placeholder="City, area" value={form.city} onChange={e => set("city", e.target.value)} />
          </div>

          <div style={styles.formSubhead}>Specifications</div>
          <div style={styles.formGrid}>
            <select style={styles.formInput} value={form.movement} onChange={e => set("movement", e.target.value)}>
              {["Automatic", "Manual", "Quartz", "Solar"].map(m => <option key={m}>{m}</option>)}
            </select>
            <input style={styles.formInput} placeholder="Case size (e.g. 40mm)" value={form.caseSize} onChange={e => set("caseSize", e.target.value)} />
            <select style={styles.formInput} value={form.caseMaterial} onChange={e => set("caseMaterial", e.target.value)}>
              {["Stainless Steel", "Titanium", "Gold", "Two-Tone", "Resin", "Bronze", "Ceramic"].map(m => <option key={m}>{m}</option>)}
            </select>
            <select style={styles.formInput} value={form.strap} onChange={e => set("strap", e.target.value)}>
              {["Leather", "Bracelet (Steel)", "Bracelet (Jubilee)", "Rubber", "Canvas", "NATO", "Resin"].map(m => <option key={m}>{m}</option>)}
            </select>
            <input style={styles.formInput} placeholder="Water resistance (e.g. 100m)" value={form.waterResist} onChange={e => set("waterResist", e.target.value)} />
            <select style={styles.formInput} value={form.crystal} onChange={e => set("crystal", e.target.value)}>
              {["Sapphire", "Mineral", "Hardlex", "Hesalite", "Acrylic"].map(m => <option key={m}>{m}</option>)}
            </select>
            <input style={styles.formInput} placeholder="Dial color" value={form.dialColor} onChange={e => set("dialColor", e.target.value)} />
          </div>

          <textarea style={styles.formTextarea} placeholder="Describe condition, included papers/box, service history..."
            value={form.desc} onChange={e => set("desc", e.target.value)} rows={3} />
          <label style={styles.checkboxRow}>
            <input type="checkbox" checked={form.papers} onChange={e => set("papers", e.target.checked)} />
            Includes box & papers
          </label>
          <button style={{ ...styles.primaryBtn, width: "100%", marginTop: 14 }} onClick={submit}>Publish listing</button>
        </div>
      </div>
    </div>
  );
}

// ---------- Styles ----------
const fontImport = `
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&display=swap');
  @media (max-width: 760px) {
    .secundo-main-layout { flex-direction: column !important; }
    .secundo-sidebar { width: 100% !important; position: static !important; }
  }
`;

const ink = "#14161a";
const paper = "#fafafa";
const accent = "#1d6f5c";
const accentDeep = "#13483c";
const accentSoft = "#e6f2ef";
const line = "#e7e7e5";

const styles = {
  app: { fontFamily: "'Inter', sans-serif", background: paper, color: ink, minHeight: "100%", display: "flex", flexDirection: "column" },
  header: { borderBottom: `1px solid ${line}`, background: paper, position: "sticky", top: 0, zIndex: 10 },
  headerInner: { maxWidth: 1100, margin: "0 auto", padding: "16px 24px", display: "flex", justifyContent: "space-between", alignItems: "center" },
  brandMark: { display: "flex", alignItems: "center", gap: 10 },
  brandDial: { width: 22, height: 22, borderRadius: "50%", border: `2.5px solid ${ink}`, position: "relative", display: "inline-block", boxShadow: `inset 0 0 0 2px ${paper}` },
  brandName: { fontFamily: "'Space Grotesk', sans-serif", fontWeight: 600, fontSize: 22, letterSpacing: "-0.01em" },
  headerActions: { display: "flex", gap: 10 },
  ghostBtn: { background: "transparent", border: `1px solid ${line}`, borderRadius: 999, padding: "9px 18px", fontSize: 14, cursor: "pointer", color: ink, fontFamily: "inherit", transition: "background 0.15s" },
  primaryBtn: { background: accent, color: "#fff", border: "none", borderRadius: 999, padding: "9px 18px", fontSize: 14, cursor: "pointer", fontFamily: "inherit", fontWeight: 500, transition: "background 0.15s" },

  searchBar: { background: "#fff", borderBottom: `1px solid ${line}` },
  searchBarInner: { maxWidth: 1280, margin: "0 auto", padding: "14px 24px", display: "flex", gap: 16, alignItems: "center" },
  searchInput: { flex: 1, padding: "11px 14px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 14, fontFamily: "inherit", outline: "none" },
  viewToggle: { display: "flex", border: `1px solid ${line}`, borderRadius: 8, overflow: "hidden", flexShrink: 0 },
  toggleBtn: { padding: "9px 16px", border: "none", background: "#fff", cursor: "pointer", fontSize: 14, fontFamily: "inherit" },
  toggleBtnActive: { background: ink, color: paper },

  mainLayout: { maxWidth: 1280, margin: "0 auto", padding: "24px", flex: 1, width: "100%", boxSizing: "border-box", display: "flex", gap: 28, alignItems: "flex-start" },
  sidebar: { width: 248, flexShrink: 0, background: "#fff", border: `1px solid ${line}`, borderRadius: 12, padding: "18px 18px 22px", position: "sticky", top: 88 },
  sidebarHeadRow: { display: "flex", justifyContent: "space-between", alignItems: "baseline", marginBottom: 14 },
  sidebarTitle: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 17 },
  resetLink: { background: "none", border: "none", color: accentDeep, fontSize: 12.5, cursor: "pointer", fontFamily: "inherit", padding: 0 },
  filterGroup: { marginBottom: 16 },
  filterLabel: { display: "block", fontSize: 11.5, textTransform: "uppercase", letterSpacing: "0.04em", color: "#8a8568", marginBottom: 6 },
  filterSelect: { width: "100%", padding: "9px 10px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 13.5, fontFamily: "inherit", background: "#fff", boxSizing: "border-box" },
  filterInput: { width: "100%", padding: "9px 10px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 13.5, fontFamily: "inherit", outline: "none", boxSizing: "border-box" },
  filterSlider: { width: "100%", accentColor: accent },

  results: { flex: 1, minWidth: 0 },
  resultsHeader: { marginBottom: 14 },
  resultsCount: { fontSize: 13, color: "#6b6456", textTransform: "uppercase", letterSpacing: "0.06em" },

  grid: { display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(240px, 1fr))", gap: 18 },
  card: { background: "#fff", border: `1px solid ${line}`, borderRadius: 16, overflow: "hidden", cursor: "pointer", transition: "transform 0.15s, box-shadow 0.15s" },
  cardPhoto: { height: 150, position: "relative" },
  cardDistance: { position: "absolute", bottom: 8, right: 8, background: "rgba(0,0,0,0.55)", color: "#fff", fontSize: 11, padding: "3px 8px", borderRadius: 20 },
  cardBody: { padding: "12px 14px 14px" },
  cardTitleRow: { display: "flex", justifyContent: "space-between", alignItems: "baseline" },
  cardBrand: { fontSize: 12, textTransform: "uppercase", letterSpacing: "0.05em", color: accentDeep, fontWeight: 600 },
  cardPrice: { fontFamily: "'Space Grotesk', sans-serif", fontWeight: 600, fontSize: 17 },
  cardModel: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 18, marginTop: 2 },
  cardMeta: { fontSize: 12.5, color: "#6b6456", marginTop: 6 },
  cardSpecRow: { display: "flex", gap: 6, flexWrap: "wrap", marginTop: 8 },
  miniTag: { fontSize: 10.5, background: "#f1ede2", color: "#5c5644", padding: "2px 7px", borderRadius: 10 },
  empty: { gridColumn: "1/-1", textAlign: "center", padding: "60px 20px", color: "#6b6456", fontSize: 14 },

  mapWrap: { border: `1px solid ${line}`, borderRadius: 12, overflow: "hidden", background: "#fff" },
  mapCanvas: { position: "relative", height: 480, background: "linear-gradient(180deg,#eef0e9,#e3e7da)", backgroundImage: "linear-gradient(#d9dccb 1px, transparent 1px), linear-gradient(90deg, #d9dccb 1px, transparent 1px)", backgroundSize: "40px 40px" },
  mapPin: { position: "absolute", transform: "translate(-50%,-100%)", background: ink, color: paper, border: "none", borderRadius: "16px 16px 16px 4px", padding: "5px 9px", fontSize: 11, fontWeight: 600, cursor: "pointer" },
  mapPinPrice: {},
  mapCenter: { position: "absolute", left: "50%", top: "50%", width: 12, height: 12, borderRadius: "50%", background: accent, border: "3px solid #fff", boxShadow: "0 0 0 2px " + accent, transform: "translate(-50%,-50%)" },
  mapNote: { position: "absolute", bottom: 10, left: 12, fontSize: 11, color: "#8a8568", background: "rgba(255,255,255,0.8)", padding: "3px 8px", borderRadius: 6 },

  overlay: { position: "fixed", inset: 0, background: "rgba(20,18,14,0.45)", display: "flex", alignItems: "center", justifyContent: "center", zIndex: 50, padding: 16 },
  modal: { background: "#fff", borderRadius: 16, maxWidth: 460, width: "100%", maxHeight: "88vh", overflowY: "auto", position: "relative" },
  closeBtn: { position: "absolute", top: 12, right: 14, background: "rgba(0,0,0,0.06)", border: "none", borderRadius: "50%", width: 30, height: 30, fontSize: 18, cursor: "pointer", zIndex: 2 },
  modalPhoto: { height: 200, borderRadius: "16px 16px 0 0" },
  modalBody: { padding: "18px 22px 22px" },
  modalTitleRow: { display: "flex", justifyContent: "space-between", alignItems: "flex-start" },
  modalBrand: { fontSize: 12, textTransform: "uppercase", letterSpacing: "0.05em", color: accentDeep, fontWeight: 600 },
  modalModel: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 24, marginTop: 2 },
  modalPrice: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 22, fontWeight: 600 },
  tagRow: { display: "flex", gap: 8, flexWrap: "wrap", marginTop: 12 },
  tag: { fontSize: 12, background: "#f1ede2", padding: "4px 10px", borderRadius: 20, color: "#5c5644" },
  tagAccent: { fontSize: 12, background: accentSoft, padding: "4px 10px", borderRadius: 20, color: accentDeep, fontWeight: 600 },
  modalDesc: { fontSize: 14, lineHeight: 1.6, marginTop: 14, color: "#3c382f" },
  specGrid: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: "10px 16px", marginTop: 16, padding: "14px 0", borderTop: `1px solid ${line}`, borderBottom: `1px solid ${line}` },
  specItem: { display: "flex", flexDirection: "column", gap: 2 },
  specLabel: { fontSize: 11, textTransform: "uppercase", letterSpacing: "0.04em", color: "#9a937e" },
  specValue: { fontSize: 13.5, color: ink, fontWeight: 500 },
  sellerRow: { display: "flex", alignItems: "center", gap: 10, marginTop: 16, paddingTop: 16, borderTop: `1px solid ${line}` },
  avatar: { width: 36, height: 36, borderRadius: "50%", background: ink, color: paper, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 13, fontWeight: 600 },
  sellerName: { fontSize: 14, fontWeight: 500 },
  sellerRating: { fontSize: 12.5, color: "#6b6456" },

  threadBox: { marginTop: 16, display: "flex", flexDirection: "column", gap: 8, maxHeight: 180, overflowY: "auto" },
  threadEmpty: { fontSize: 13, color: "#8a8568", padding: "8px 0" },
  bubble: { fontSize: 13.5, padding: "8px 12px", borderRadius: 14, maxWidth: "80%", lineHeight: 1.4 },
  bubbleYou: { background: ink, color: paper, alignSelf: "flex-end", borderBottomRightRadius: 4 },
  bubbleSeller: { background: "#f1ede2", color: ink, alignSelf: "flex-start", borderBottomLeftRadius: 4 },
  composeRow: { display: "flex", gap: 8, marginTop: 12 },
  composeInput: { flex: 1, padding: "10px 12px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 13.5, fontFamily: "inherit", outline: "none" },

  drawer: { background: "#fff", borderRadius: 16, maxWidth: 420, width: "100%", maxHeight: "80vh", overflowY: "auto", padding: "18px 20px" },
  drawerHeader: { display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 12 },
  drawerTitle: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 20 },
  threadRow: { display: "flex", gap: 12, alignItems: "center", padding: "10px 4px", borderBottom: `1px solid ${line}`, cursor: "pointer" },
  threadPhoto: { width: 44, height: 44, borderRadius: 10, flexShrink: 0 },
  threadName: { fontSize: 14, fontWeight: 500 },
  threadLast: { fontSize: 12.5, color: "#6b6456", marginTop: 2 },

  formTitle: { fontFamily: "'Space Grotesk', sans-serif", fontSize: 22, marginBottom: 14 },
  formSubhead: { fontSize: 12, textTransform: "uppercase", letterSpacing: "0.05em", color: "#9a937e", margin: "16px 0 8px" },
  formGrid: { display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10 },
  formInput: { padding: "10px 12px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 13.5, fontFamily: "inherit", outline: "none" },
  formTextarea: { width: "100%", marginTop: 10, padding: "10px 12px", border: `1px solid ${line}`, borderRadius: 8, fontSize: 13.5, fontFamily: "inherit", outline: "none", boxSizing: "border-box", resize: "vertical" },
  checkboxRow: { display: "flex", alignItems: "center", gap: 8, fontSize: 13.5, marginTop: 12, color: "#3c382f" },

  toast: { position: "fixed", bottom: 24, left: "50%", transform: "translateX(-50%)", background: ink, color: paper, padding: "10px 18px", borderRadius: 10, fontSize: 13.5, zIndex: 100 },

  footer: { textAlign: "center", fontSize: 12, color: "#8a8568", padding: "24px", borderTop: `1px solid ${line}` },
};
