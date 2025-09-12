import React, { useEffect, useMemo, useRef, useState } from "react";
import { Calendar, Download, FileDown, Plus, Save, Search, Trash2, Upload, Users, X, Info, Send } from "lucide-react";
import * as XLSX from "xlsx";
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Textarea } from "@/components/ui/textarea";
import { Switch } from "@/components/ui/switch";
import { Badge } from "@/components/ui/badge";
import { Separator } from "@/components/ui/separator";

// ------------------------------------------------------------
// App web – Pointages Intérim (v3)
// ------------------------------------------------------------
// Ajouts pris en compte :
// - Entrée décimale avec virgules (7,5) ou points (7.5)
// - Sélection de la semaine à POINTER et de la semaine à ENVOYER
// - Pré‑création d'ouvriers & d'agences via import Excel d'initialisation
// - Gestion des CHANTIERS (CRUD) et sélection du chantier par ligne
// - Gestion des AGENCES (CRUD)
// - Export Excel global et par agence basé sur la semaine d'envoi
// - Import Excel/CSV (ajout de lignes) et Import "Initialiser depuis Excel" (remplace la base)
// ------------------------------------------------------------

export type DayKey = "mon" | "tue" | "wed" | "thu" | "fri" | "sat" | "sun";
export type InterimRow = {
  id: string;
  agency: string;
  worker: string;
  mission?: string;
  chantier?: string;
  days: Record<DayKey, number | "">;
  notes?: string;
  archived?: boolean;
};

const DAY_LABELS: Record<DayKey, string> = {
  mon: "Lun",
  tue: "Mar",
  wed: "Mer",
  thu: "Jeu",
  fri: "Ven",
  sat: "Sam",
  sun: "Dim",
};

function isoWeekStart(date: Date) {
  const d = new Date(date);
  const day = (d.getDay() + 6) % 7; // lundi=0
  d.setDate(d.getDate() - day);
  d.setHours(0, 0, 0, 0);
  return d;
}
function addDays(d: Date, n: number) { const x = new Date(d); x.setDate(x.getDate() + n); return x; }
function fmt(d: Date) { return d.toLocaleDateString("fr-FR"); }
function uid() { return Math.random().toString(36).slice(2, 9); }
function parseNumberOrEmpty(v: any): number | "" { if (v === undefined || v === null || v === "") return ""; const n = Number(String(v).replace(",", ".")); return isFinite(n) ? n : ""; }

const LS_KEY = "pointages_interim_v3_rows";
const LS_META = "pointages_interim_v3_meta";

const MAX_DAILY = 10; // h
const MAX_WEEKLY = 48; // h

// Valeurs de départ (éditables dans les écrans de gestion)
const DEFAULT_AGENCIES = ["SAMSIC", "BPS", "SOVITRAT", "YES"];
const DEFAULT_CHANTIERS = ["M91", "M96", "M65", "M01"];

export default function App() {
  const [rows, setRows] = useState<InterimRow[]>([]);
  const [weekStart, setWeekStart] = useState<Date>(() => isoWeekStart(new Date())); // semaine à pointer
  const [weekSend, setWeekSend] = useState<Date>(() => isoWeekStart(new Date()));  // semaine à envoyer
  const [search, setSearch] = useState("");
  const [agencyFilter, setAgencyFilter] = useState<string[]>([]);
  const [chantierFilter, setChantierFilter] = useState<string[]>([]);
  const [lock, setLock] = useState(false);
  const [emailSentCounter, setEmailSentCounter] = useState(0);
  const [agencies, setAgencies] = useState<string[]>(DEFAULT_AGENCIES);
  const [chantiers, setChantiers] = useState<string[]>(DEFAULT_CHANTIERS);
  const [newAgency, setNewAgency] = useState("");
  const [newChantier, setNewChantier] = useState("");

  // File inputs
  const addFileRef = useRef<HTMLInputElement | null>(null);
  const initFileRef = useRef<HTMLInputElement | null>(null);

  // Load from LS
  useEffect(() => {
    const ls = localStorage.getItem(LS_KEY);
    const meta = localStorage.getItem(LS_META);
    if (ls) {
      try { setRows(JSON.parse(ls)); } catch {}
    }
    if (meta) {
      try {
        const m = JSON.parse(meta);
        if (m.weekStart) setWeekStart(new Date(m.weekStart));
        if (m.weekSend) setWeekSend(new Date(m.weekSend));
        if (m.emailSentCounter) setEmailSentCounter(Number(m.emailSentCounter) || 0);
        if (m.agencies?.length) setAgencies(m.agencies);
        if (m.chantiers?.length) setChantiers(m.chantiers);
      } catch {}
    }
  }, []);

  // Persist
  useEffect(() => { localStorage.setItem(LS_KEY, JSON.stringify(rows)); }, [rows]);
  useEffect(() => { localStorage.setItem(LS_META, JSON.stringify({ weekStart, weekSend, emailSentCounter, agencies, chantiers })); }, [weekStart, weekSend, emailSentCounter, agencies, chantiers]);

  // Derived
  const weekDays = useMemo(() => {
    const start = isoWeekStart(weekStart);
    return (Object.keys(DAY_LABELS) as DayKey[]).map((k, i) => ({ key: k, date: addDays(start, i) }));
  }, [weekStart]);

  const allAgencies = useMemo(() => Array.from(new Set([...(agencies||[]), ...rows.map(r => r.agency).filter(Boolean)])).sort(), [agencies, rows]);
  const allChantiers = useMemo(() => Array.from(new Set([...(chantiers||[]), ...rows.map(r => r.chantier||"")])).filter(Boolean).sort(), [chantiers, rows]);

  const filtered = useMemo(() => rows.filter(r => {
    if (r.archived) return false;
    const okAgency = agencyFilter.length ? agencyFilter.includes(r.agency) : true;
    const okChantier = chantierFilter.length ? (r.chantier ? chantierFilter.includes(r.chantier) : false) : true;
    const okSearch = search ? (r.worker + " " + (r.mission || "") + " " + r.agency + " " + (r.chantier||"")).toLowerCase().includes(search.toLowerCase()) : true;
    return okAgency && okChantier && okSearch;
  }), [rows, agencyFilter, chantierFilter, search]);

  // Helpers
  function weeklyTotal(r: InterimRow) {
    return (Object.keys(DAY_LABELS) as DayKey[]).reduce((s, k) => s + (typeof r.days[k] === "number" ? (r.days[k] as number) : 0), 0);
  }
  function dayHasError(val: number | "") { if (val === "") return false; return typeof val === "number" && val > MAX_DAILY; }
  function rowHasWeeklyError(r: InterimRow) { return weeklyTotal(r) > MAX_WEEKLY; }

  // Mutations
  function addRow(pref?: Partial<InterimRow>) {
    const base: InterimRow = {
      id: uid(),
      agency: pref?.agency || "",
      worker: pref?.worker || "",
      mission: pref?.mission || "",
      chantier: pref?.chantier || "",
      days: { mon: "", tue: "", wed: "", thu: "", fri: "", sat: "", sun: "" },
      notes: pref?.notes || "",
    };
    setRows(r => [base, ...r]);
  }
  function setCell(id: string, key: keyof InterimRow | DayKey, value: any) {
    if (lock) return;
    setRows(list => list.map(r => {
      if (r.id !== id) return r;
      if (key in r.days) {
        return { ...r, days: { ...r.days, [key as DayKey]: parseNumberOrEmpty(value) } };
      }
      // @ts-ignore
      return { ...r, [key]: value };
    }));
  }
  function removeRow(id: string) { if (confirm("Supprimer cette ligne ?")) setRows(list => list.filter(r => r.id !== id)); }

  // Imports/Exports
  function exportToXlsx(perAgency = false) {
    const start = isoWeekStart(weekSend); // export basé sur semaine d'envoi
    const makeSheetData = (subset: InterimRow[]) => {
      const header = [["Semaine envoyée", fmt(start), "au", fmt(addDays(start, 6))], []];
      const cols = ["Agence", "Intérimaire", "Mission", "Chantier", ...(Object.keys(DAY_LABELS) as DayKey[]).map(k => DAY_LABELS[k] + " (h)"), "Total", "Notes"];
      const body = subset.map(r => [
        r.agency,
        r.worker,
        r.mission || "",
        r.chantier || "",
        ...(Object.keys(DAY_LABELS) as DayKey[]).map(k => (r.days[k] === "" ? "" : r.days[k])),
        weeklyTotal(r),
        r.notes || "",
      ]);
      return [...header, cols, ...body];
    };

    if (perAgency) {
      const groups: Record<string, InterimRow[]> = {};
      for (const r of rows) {
        if (r.archived) continue;
        const k = r.agency || "(Sans agence)";
        groups[k] = groups[k] || [];
        groups[k].push(r);
      }
      Object.entries(groups).forEach(([agency, subset]) => {
        const wb = XLSX.utils.book_new();
        const ws = XLSX.utils.aoa_to_sheet(makeSheetData(subset));
        XLSX.utils.book_append_sheet(wb, ws, "Pointages");
        XLSX.writeFile(wb, `Pointages_${agency}_${fmt(start).replaceAll("/","-")}.xlsx`);
      });
      return;
    }

    const wb = XLSX.utils.book_new();
    const ws = XLSX.utils.aoa_to_sheet(makeSheetData(rows.filter(r => !r.archived)));
    XLSX.utils.book_append_sheet(wb, ws, "Pointages");
    XLSX.writeFile(wb, `Pointages_${fmt(start).replaceAll("/","-")}.xlsx`);
  }

  // Ajout de lignes (merge)
  function importAddFromFile(file: File) {
    const reader = new FileReader();
    reader.onload = (e) => {
      const data = new Uint8Array(e.target?.result as ArrayBuffer);
      const wb = XLSX.read(data, { type: "array" });
      const ws = wb.Sheets[wb.SheetNames[0]];
      const json: any[] = XLSX.utils.sheet_to_json(ws, { defval: "" });

      const mapped: InterimRow[] = json.map((row) => {
        const get = (keys: string[]) => keys.map(k => row[k]).find(v => v !== undefined && v !== "");
        const getH = (keys: string[]) => parseNumberOrEmpty(get(keys));
        return {
          id: uid(),
          agency: (get(["Agence", "agency"]) || "") + "",
          worker: (get(["Intérimaire", "Nom", "worker"]) || "") + "",
          mission: (get(["Mission"]) || "") + "",
          chantier: (get(["Chantier", "chantier"]) || "") + "",
          days: {
            mon: getH(["Lun", "Mon", "Lundi"]) || "",
            tue: getH(["Mar", "Tue", "Mardi"]) || "",
            wed: getH(["Mer", "Wed", "Mercredi"]) || "",
            thu: getH(["Jeu", "Thu", "Jeudi"]) || "",
            fri: getH(["Ven", "Fri", "Vendredi"]) || "",
            sat: getH(["Sam", "Sat", "Samedi"]) || "",
            sun: getH(["Dim", "Sun", "Dimanche"]) || "",
          },
          notes: (get(["Notes"]) || "") + "",
        };
      });
      setRows(prev => [...mapped, ...prev]);
    };
    reader.readAsArrayBuffer(file);
  }

  // Initialisation complète (remplace ouvriers + agences + chantiers s'ils existent en feuilles dédiées)
  function importInitFromFile(file: File) {
    const reader = new FileReader();
    reader.onload = (e) => {
      const data = new Uint8Array(e.target?.result as ArrayBuffer);
      const wb = XLSX.read(data, { type: "array" });

      // 1) Ouvriers: feuille "Ouvriers" (colonnes: Agence, Intérimaire[, Mission][, Chantier])
      let newRows: InterimRow[] = [];
      const ouvSheet = wb.Sheets["Ouvriers"] || wb.Sheets[wb.SheetNames.find(n => n.toLowerCase().includes("ouvrier")) || ""];
      if (ouvSheet) {
        const j: any[] = XLSX.utils.sheet_to_json(ouvSheet, { defval: "" });
        newRows = j.map((row) => ({
          id: uid(),
          agency: String(row["Agence"] || ""),
          worker: String(row["Intérimaire"] || row["Nom"] || ""),
          mission: row["Mission"] ? String(row["Mission"]) : "",
          chantier: row["Chantier"] ? String(row["Chantier"]) : "",
          days: { mon: "", tue: "", wed: "", thu: "", fri: "", sat: "", sun: "" },
          notes: "",
        }));
      }

      // 2) Agences: feuille "Agences" (colonne: Agence)
      let newAgencies: string[] | null = null;
      const agSheet = wb.Sheets["Agences"] || wb.Sheets[wb.SheetNames.find(n => n.toLowerCase().includes("agence")) || ""];
      if (agSheet) {
        const j: any[] = XLSX.utils.sheet_to_json(agSheet, { defval: "" });
        newAgencies = j.map(r => String(r["Agence"] || r["agency"] || "")).filter(Boolean);
      }

      // 3) Chantiers: feuille "Chantiers" (colonne: Chantier)
      let newChantiers: string[] | null = null;
      const chSheet = wb.Sheets["Chantiers"] || wb.Sheets[wb.SheetNames.find(n => n.toLowerCase().includes("chantier")) || ""];
      if (chSheet) {
        const j: any[] = XLSX.utils.sheet_to_json(chSheet, { defval: "" });
        newChantiers = j.map(r => String(r["Chantier"] || r["chantier"] || "")).filter(Boolean);
      }

      // fallback: si pas de feuilles dédiées, on utilise la 1ère feuille comme liste ouvriers
      if (!newRows.length) {
        const ws = wb.Sheets[wb.SheetNames[0]];
        const j: any[] = XLSX.utils.sheet_to_json(ws, { defval: "" });
        newRows = j.map((row) => ({
          id: uid(),
          agency: String(row["Agence"] || row["agency"] || ""),
          worker: String(row["Intérimaire"] || row["Nom"] || row["worker"] || ""),
          mission: String(row["Mission"] || ""),
          chantier: String(row["Chantier"] || row["chantier"] || ""),
          days: { mon: "", tue: "", wed: "", thu: "", fri: "", sat: "", sun: "" },
          notes: "",
        }));
      }

      setRows(newRows);
      if (newAgencies) setAgencies(Array.from(new Set(newAgencies)));
      if (newChantiers) setChantiers(Array.from(new Set(newChantiers)));
      alert(`Base initialisée : ${newRows.length} ouvriers${newAgencies ? ", " + newAgencies.length + " agences" : ""}${newChantiers ? ", " + newChantiers.length + " chantiers" : ""}.`);
    };
    reader.readAsArrayBuffer(file);
  }

  function simulateSendEmails() {
    const selected = filtered;
    if (!selected.length) { alert("Aucune ligne à envoyer selon les filtres."); return; }
    const perAgency: Record<string, number> = {};
    selected.forEach(r => { const key = r.agency || "(Sans agence)"; perAgency[key] = (perAgency[key] || 0) + 1; });
    const sent = Object.keys(perAgency).length; // 1 email par agence
    setEmailSentCounter(c => c + sent);
    alert(`Simulation d'envoi : ${sent} emails pour la semaine du ${fmt(isoWeekStart(weekSend))}. Total envoyés: ${emailSentCounter + sent}`);
  }

  // Totaux globaux
  const totalsPerDay = useMemo(() => {
    const t: Record<DayKey, number> = { mon: 0, tue: 0, wed: 0, thu: 0, fri: 0, sat: 0, sun: 0 };
    for (const r of filtered) {
      (Object.keys(DAY_LABELS) as DayKey[]).forEach(k => {
        const v = r.days[k];
        if (typeof v === "number") t[k] += v;
      });
    }
    return t;
  }, [filtered]);
  const totalWeekAll = useMemo(() => Object.values(totalsPerDay).reduce((a, b) => a + b, 0), [totalsPerDay]);

  return (
    <div className="min-h-screen w-full bg-gradient-to-b from-slate-50 to-white p-4 md:p-8">
      <div className="mx-auto max-w-7xl space-y-6">
        {/* HEADER */}
        <div className="flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
          <div>
            <h1 className="text-2xl md:text-3xl font-semibold tracking-tight">Pointages Intérim</h1>
            <p className="text-slate-500">Pointer : semaine du {fmt(isoWeekStart(weekStart))} • Envoyer : semaine du {fmt(isoWeekStart(weekSend))}</p>
          </div>
          <div className="flex items-center gap-3">
            <Button onClick={() => addRow()}><Plus className="mr-2 h-4 w-4"/>Nouvelle ligne</Button>
            <Button variant={lock ? "secondary" : "default"} onClick={() => setLock(v => !v)}><Info className="mr-2 h-4 w-4" /> {lock ? "Déverrouiller" : "Verrouiller"}</Button>
            <Button variant="secondary" onClick={() => exportToXlsx(false)}><Download className="mr-2 h-4 w-4" /> Export Excel (global)</Button>
            <Button variant="secondary" onClick={() => exportToXlsx(true)}><FileDown className="mr-2 h-4 w-4" /> Export par Agence</Button>
            <Button onClick={simulateSendEmails}><Send className="mr-2 h-4 w-4"/>Simuler envoi</Button>
          </div>
        </div>

        {/* PARAMÈTRES & IMPORTS */}
        <Card className="shadow-sm">
          <CardHeader className="pb-2">
            <CardTitle className="text-lg">Paramètres semaine & filtres</CardTitle>
            <CardDescription>Choisissez les semaines et filtrez par agence/chantier/nom.</CardDescription>
          </CardHeader>
          <CardContent className="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div className="space-y-2">
              <Label>Semaine à pointer</Label>
              <div className="flex items-center gap-2">
                <Input type="date" value={new Date(weekStart.getTime() - weekStart.getTimezoneOffset()*60000).toISOString().slice(0,10)} onChange={(e) => setWeekStart(isoWeekStart(new Date(e.target.value)))} />
                <Button variant="outline" onClick={() => setWeekStart(isoWeekStart(new Date()))}><Calendar className="h-4 w-4 mr-2"/>Aujourd'hui</Button>
              </div>
            </div>
            <div className="space-y-2">
              <Label>Semaine à envoyer</Label>
              <div className="flex items-center gap-2">
                <Input type="date" value={new Date(weekSend.getTime() - weekSend.getTimezoneOffset()*60000).toISOString().slice(0,10)} onChange={(e) => setWeekSend(isoWeekStart(new Date(e.target.value)))} />
              </div>
            </div>
            <div className="space-y-2">
              <Label>Recherche</Label>
              <div className="relative">
                <Search className="absolute left-2 top-2.5 h-4 w-4 text-slate-400"/>
                <Input className="pl-8" placeholder="Nom, mission, agence, chantier…" value={search} onChange={e => setSearch(e.target.value)} />
              </div>
            </div>
          </CardContent>
          <CardFooter className="flex flex-wrap items-center justify-between gap-3">
            <div className="flex items-center gap-3 flex-wrap">
              <div>
                <Label className="mr-2">Filtre Agence</Label>
                <div className="inline-flex flex-wrap gap-2">
                  {allAgencies.map(a => (
                    <Badge key={a} variant={agencyFilter.includes(a) ? "default" : "secondary"} className="cursor-pointer" onClick={() => setAgencyFilter(f => f.includes(a) ? f.filter(x => x !== a) : [...f, a])}>{a}</Badge>
                  ))}
                  {agencyFilter.length > 0 && (
                    <Button size="sm" variant="ghost" onClick={() => setAgencyFilter([])}>Réinitialiser</Button>
                  )}
                </div>
              </div>
              <div>
                <Label className="mr-2">Filtre Chantier</Label>
                <div className="inline-flex flex-wrap gap-2">
                  {allChantiers.map(c => (
                    <Badge key={c} variant={chantierFilter.includes(c) ? "default" : "secondary"} className="cursor-pointer" onClick={() => setChantierFilter(f => f.includes(c) ? f.filter(x => x !== c) : [...f, c])}>{c}</Badge>
                  ))}
                  {chantierFilter.length > 0 && (
                    <Button size="sm" variant="ghost" onClick={() => setChantierFilter([])}>Réinitialiser</Button>
                  )}
                </div>
              </div>
            </div>
            <div className="flex items-center gap-3 flex-wrap">
              <input ref={addFileRef} type="file" accept=".xlsx,.xls,.csv" className="hidden" onChange={(e) => e.target.files && importAddFromFile(e.target.files[0])} />
              <Button variant="outline" onClick={() => addFileRef.current?.click()}><Upload className="h-4 w-4 mr-2"/>Importer (ajouter)</Button>

              <input ref={initFileRef} type="file" accept=".xlsx,.xls" className="hidden" onChange={(e) => e.target.files && importInitFromFile(e.target.files[0])} />
              <Button variant="outline" onClick={() => initFileRef.current?.click()} title="Feuilles attendues: Ouvriers, Agences, Chantiers"><Upload className="h-4 w-4 mr-2"/>Initialiser depuis Excel</Button>

              <Button variant="outline" onClick={() => { if(confirm('Vider toutes les lignes ?')) setRows([])}}><Trash2 className="h-4 w-4 mr-2"/>Tout effacer</Button>
              <div className="flex items-center gap-2">
                <Switch checked={lock} onCheckedChange={setLock} id="lock" />
                <Label htmlFor="lock">Verrouiller la saisie</Label>
              </div>
            </div>
          </CardFooter>
        </Card>

        {/* GESTION AGENCES & CHANTIERS */}
        <div className="grid md:grid-cols-2 gap-6">
          <Card className="shadow-sm">
            <CardHeader className="pb-2"><CardTitle>Agences (CRUD)</CardTitle><CardDescription>Ajoutez/supprimez des agences disponibles.</CardDescription></CardHeader>
            <CardContent>
              <div className="flex gap-2 mb-3">
                <Input value={newAgency} onChange={e=>setNewAgency(e.target.value)} placeholder="Nouvelle agence"/>
                <Button onClick={()=>{ if(newAgency){ setAgencies(a=>Array.from(new Set([...a, newAgency]))); setNewAgency(""); } }}><Plus className="mr-2 h-4 w-4"/>Ajouter</Button>
              </div>
              <div className="flex flex-wrap gap-2">
                {agencies.map(a => (
                  <Badge key={a} className="flex items-center gap-1">{a}<X className="h-3 w-3 cursor-pointer" onClick={()=>setAgencies(agencies.filter(x=>x!==a))}/></Badge>
                ))}
              </div>
            </CardContent>
          </Card>

          <Card className="shadow-sm">
            <CardHeader className="pb-2"><CardTitle>Chantiers (CRUD)</CardTitle><CardDescription>Gérez la liste des chantiers référencés.</CardDescription></CardHeader>
            <CardContent>
              <div className="flex gap-2 mb-3">
                <Input value={newChantier} onChange={e=>setNewChantier(e.target.value)} placeholder="Nouveau chantier (ex: M96 W01S)"/>
                <Button onClick={()=>{ if(newChantier){ setChantiers(c=>Array.from(new Set([...c, newChantier]))); setNewChantier(""); } }}><Plus className="mr-2 h-4 w-4"/>Ajouter</Button>
              </div>
              <div className="flex flex-wrap gap-2">
                {chantiers.map(c => (
                  <Badge key={c} className="flex items-center gap-1">{c}<X className="h-3 w-3 cursor-pointer" onClick={()=>setChantiers(chantiers.filter(x=>x!==c))}/></Badge>
                ))}
              </div>
            </CardContent>
          </Card>
        </div>

        {/* TABLE DE SAISIE */}
        <Card className="shadow-sm">
          <CardHeader className="pb-2">
            <CardTitle className="text-lg">Saisie hebdomadaire</CardTitle>
            <CardDescription>Renseignez les heures (décimales autorisées: 7,5) et affectez les chantiers.</CardDescription>
          </CardHeader>
          <CardContent className="overflow-x-auto">
            <div className="min-w-[1100px]">
              <div className="grid grid-cols-[170px_240px_200px_170px_repeat(7,110px)_120px_260px_60px] gap-px bg-slate-200 rounded-xl overflow-hidden">
                {/* Header Row */}
                <div className="bg-slate-50 p-3 font-medium">Agence</div>
                <div className="bg-slate-50 p-3 font-medium">Intérimaire</div>
                <div className="bg-slate-50 p-3 font-medium">Mission</div>
                <div className="bg-slate-50 p-3 font-medium">Chantier</div>
                {weekDays.map(({ key, date }) => (
                  <div key={key} className="bg-slate-50 p-3 font-medium text-center">
                    <div>{DAY_LABELS[key]}</div>
                    <div className="text-xs text-slate-500">{fmt(date)}</div>
                  </div>
                ))}
                <div className="bg-slate-50 p-3 font-medium text-center">Total</div>
                <div className="bg-slate-50 p-3 font-medium">Notes</div>
                <div className="bg-slate-50 p-3 font-medium text-center"> </div>

                {/* Data Rows */}
                {filtered.map((r) => {
                  const weeklyErr = rowHasWeeklyError(r);
                  return (
                    <React.Fragment key={r.id}>
                      <div className="bg-white p-2">
                        <select disabled={lock} value={r.agency} onChange={e => setCell(r.id, "agency", e.target.value)} className="w-full border rounded p-2">
                          <option value="">—</option>
                          {allAgencies.map(a => <option key={a} value={a}>{a}</option>)}
                        </select>
                      </div>
                      <div className="bg-white p-2">
                        <Input disabled={lock} value={r.worker} onChange={e => setCell(r.id, "worker", e.target.value)} placeholder="Nom Prénom" />
                      </div>
                      <div className="bg-white p-2">
                        <Input disabled={lock} value={r.mission || ""} onChange={e => setCell(r.id, "mission", e.target.value)} placeholder="Ex: Démolition, Plâtrerie…" />
                      </div>
                      <div className="bg-white p-2">
                        <select disabled={lock} value={r.chantier || ""} onChange={e => setCell(r.id, "chantier", e.target.value)} className="w-full border rounded p-2">
                          <option value="">—</option>
                          {allChantiers.map(c => <option key={c} value={c}>{c}</option>)}
                        </select>
                      </div>
                      {(Object.keys(DAY_LABELS) as DayKey[]).map((k) => {
                        const v = r.days[k];
                        const hasErr = dayHasError(v);
                        return (
                          <div key={k} className={`bg-white p-2 ${hasErr ? 'ring-2 ring-red-400' : ''}`}>
                            <Input disabled={lock} inputMode="decimal" placeholder="h" value={v === "" ? "" : String(v)} onChange={e => setCell(r.id, k, e.target.value)} />
                            {hasErr && <p className="text-xs text-red-500 mt-1">Max {MAX_DAILY}h/jour</p>}
                          </div>
                        );
                      })}
                      <div className={`bg-white p-2 text-center font-semibold ${weeklyErr ? 'text-red-600' : ''}`}>{weeklyTotal(r)}</div>
                      <div className="bg-white p-2">
                        <Textarea disabled={lock} value={r.notes || ""} onChange={e => setCell(r.id, "notes", e.target.value)} placeholder="Remarques, champs…" />
                      </div>
                      <div className="bg-white p-2 flex items-center justify-center">
                        <Button variant="ghost" size="icon" onClick={() => removeRow(r.id)}><X className="h-4 w-4"/></Button>
                      </div>
                    </React.Fragment>
                  );
                })}

                {/* Totals Row */}
                <div className="bg-slate-50 p-3 font-medium"> </div>
                <div className="bg-slate-50 p-3 font-medium"> </div>
                <div className="bg-slate-50 p-3 font-medium text-right">Total Jour</div>
                <div className="bg-slate-50 p-3 font-medium"> </div>
                {(Object.keys(DAY_LABELS) as DayKey[]).map((k) => (
                  <div key={k} className="bg-slate-50 p-3 text-center font-semibold">{totalsPerDay[k]}</div>
                ))}
                <div className="bg-slate-50 p-3 text-center font-bold">{totalWeekAll}</div>
                <div className="bg-slate-50 p-3"> </div>
                <div className="bg-slate-50 p-3"> </div>
              </div>
            </div>
          </CardContent>
        </Card>

        {/* Footer helper */}
        <div className="text-xs text-slate-500 text-center py-6">
          Données stockées localement (navigateur). Pour brancher un envoi d'emails réel et une base partagée (Postgres/Sheets), on ajoutera un backend API.
        </div>
      </div>
    </div>
  );
}
