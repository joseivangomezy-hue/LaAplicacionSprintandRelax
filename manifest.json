import React, { useState, useEffect, useRef } from 'react';
import { 
  Dumbbell, Timer, Plus, Trash2, Save, ChevronLeft, ChevronRight, 
  ArrowLeft, X, Clock, Waves, Zap, FileText, Calendar, Activity, 
  Trophy, StickyNote, CheckCircle2, Moon, Smartphone
} from 'lucide-react';

const App = () => {
  const [activeTab, setActiveTab] = useState('calendario'); 
  const [editingDay, setEditingDay] = useState(null); 
  const [isStandalone, setIsStandalone] = useState(true);
  const [showInstallBanner, setShowInstallBanner] = useState(false);
  
  const [currentMesocycle, setCurrentMesocycle] = useState(1);
  const [currentWeek, setCurrentWeek] = useState(1);
  const [plan, setPlan] = useState({}); 
  
  const [sessionType, setSessionType] = useState('carrera');
  const [sessionName, setSessionName] = useState('');
  const [intervals, setIntervals] = useState([]); 
  const [swimBlocks, setSwimBlocks] = useState([]);
  const [ropeBlocks, setRopeBlocks] = useState([]);
  const [exercises, setExercises] = useState([]);

  const days = ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo'];
  const weekScrollRef = useRef(null);
  const startWeek = (currentMesocycle - 1) * 4 + 1;

  // Configuración de App Móvil Nativa (PWA)
  useEffect(() => {
    const isPWA = window.matchMedia('(display-mode: standalone)').matches || window.navigator.standalone;
    setIsStandalone(isPWA);
    setShowInstallBanner(!isPWA);

    // Inyección de Metaetiquetas para iOS y Android
    const metaTags = {
      'viewport': 'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover',
      'apple-mobile-web-app-capable': 'yes',
      'apple-mobile-web-app-status-bar-style': 'black-translucent',
      'theme-color': '#0A0A0C',
      'mobile-web-app-capable': 'yes'
    };

    Object.entries(metaTags).forEach(([name, content]) => {
      let meta = document.querySelector(`meta[name="${name}"]`);
      if (!meta) {
        meta = document.createElement('meta');
        meta.name = name;
        document.head.appendChild(meta);
      } else {
        meta.content = content;
      }
    });

    // Deshabilitar comportamientos web (Zoom, Overscroll) para sensación nativa
    document.body.style.overscrollBehavior = 'none';
    document.documentElement.style.overscrollBehavior = 'none';
  }, []);

  const handlePrevMeso = () => {
    if (currentMesocycle > 1) {
      const newMeso = currentMesocycle - 1;
      setCurrentMesocycle(newMeso);
      setCurrentWeek((newMeso - 1) * 4 + 1);
    }
  };

  const handleNextMeso = () => {
    const newMeso = currentMesocycle + 1;
    setCurrentMesocycle(newMeso);
    setCurrentWeek((newMeso - 1) * 4 + 1);
  };

  const openEditor = (week, day) => {
    const key = `${week}-${day}`;
    const session = plan[key];
    setEditingDay({ week, day });
    
    if (session) {
      setSessionType(session.type);
      setSessionName(session.name);
      if (session.type === 'carrera') setIntervals(session.data || []);
      else if (session.type === 'fuerza') setExercises(session.data || []);
      else if (session.type === 'natacion') setSwimBlocks(session.data || []);
      else if (session.type === 'cuerda') setRopeBlocks(session.data || []);
      else if (session.type === 'descanso') {
         setIntervals([]); setExercises([]); setSwimBlocks([]); setRopeBlocks([]);
      }
    } else {
      setSessionType('carrera'); setSessionName(''); setIntervals([]); setExercises([]); setSwimBlocks([]); setRopeBlocks([]);
    }
    setActiveTab('editor');
  };

  const saveAndReturn = () => {
    if (!editingDay) return;
    let data = [];
    if (sessionType === 'carrera') data = [...intervals];
    else if (sessionType === 'fuerza') data = [...exercises];
    else if (sessionType === 'natacion') data = [...swimBlocks];
    else if (sessionType === 'cuerda') data = [...ropeBlocks];
    else if (sessionType === 'descanso') data = [];

    const key = `${editingDay.week}-${editingDay.day}`;
    setPlan({
      ...plan,
      [key]: {
        name: sessionType === 'descanso' ? 'Día de descanso' : (sessionName || `Sesión de ${sessionType}`),
        type: sessionType,
        data: data
      }
    });
    setEditingDay(null);
    setActiveTab('calendario');
  };

  const updateItemField = (blockId, itemId, field, value, type) => {
    let setter, list;
    if (type === 'fuerza') { setter = setExercises; list = exercises; }
    else if (type === 'cuerda') { setter = setRopeBlocks; list = ropeBlocks; }
    else if (type === 'natacion') { setter = setSwimBlocks; list = swimBlocks; }
    
    const newList = list.map(block => {
      if (block.id !== blockId) return block;
      return {
        ...block,
        items: block.items.map(item => item.id !== itemId ? item : { ...item, [field]: value })
      };
    });
    setter(newList);
  };

  const deleteBlock = (id, listType) => {
    if (listType === 'carrera') setIntervals(intervals.filter(i => i.id !== id));
    if (listType === 'fuerza') setExercises(exercises.filter(i => i.id !== id));
    if (listType === 'natacion') setSwimBlocks(swimBlocks.filter(i => i.id !== id));
    if (listType === 'cuerda') setRopeBlocks(ropeBlocks.filter(i => i.id !== id));
  };

  const s = {
    bg: 'bg-[#0A0A0C]',
    card: 'bg-[#141417] border-[#27272A]',
    textMain: 'text-[#F4F4F5]',
    textSec: 'text-[#A1A1AA]',
    accent: 'text-[#06B6D4]',
    inputBg: 'bg-[#18181B]',
    header: 'bg-[#0A0A0C]/90 border-[#1F1F23]'
  };

  const getSessionTheme = (type) => {
    switch (type) {
      case 'carrera': return { bg: 'bg-cyan-500/10', border: 'border-cyan-500/30', accent: 'text-cyan-400', iconBg: 'bg-cyan-500/20', icon: <Zap size={18}/> };
      case 'fuerza': return { bg: 'bg-orange-500/10', border: 'border-orange-500/30', accent: 'text-orange-400', iconBg: 'bg-orange-500/20', icon: <Dumbbell size={18}/> };
      case 'natacion': return { bg: 'bg-indigo-500/10', border: 'border-indigo-500/30', accent: 'text-indigo-400', iconBg: 'bg-indigo-500/20', icon: <Waves size={18}/> };
      case 'cuerda': return { bg: 'bg-emerald-500/10', border: 'border-emerald-500/30', accent: 'text-emerald-400', iconBg: 'bg-emerald-500/20', icon: <Timer size={18}/> };
      default: return { bg: 'bg-zinc-500/5', border: 'border-zinc-500/20', accent: 'text-zinc-400', iconBg: 'bg-zinc-500/10', icon: <Moon size={18}/> };
    }
  };

  const WeekSelector = () => {
    const weeksInMeso = [startWeek, startWeek + 1, startWeek + 2, startWeek + 3];
    return (
      <div className="space-y-4 mb-8">
        <div className="flex items-center justify-between px-2">
          <div className="flex items-center gap-2">
            <Trophy size={14} className="text-[#06B6D4]" />
            <span className="text-[10px] font-bold uppercase text-[#71717A] tracking-widest">Planificación</span>
          </div>
          <div className="flex items-center gap-3 bg-[#18181B] p-1.5 rounded-xl border border-[#27272A]">
            <button onClick={handlePrevMeso} className={`p-2 rounded-md transition-colors active:scale-90 ${currentMesocycle > 1 ? 'active:bg-zinc-800 text-white' : 'text-zinc-700'}`}><ChevronLeft size={16} /></button>
            <span className="text-[10px] font-black text-white min-w-[90px] text-center uppercase tracking-tighter">MESOCICLO {currentMesocycle}</span>
            <button onClick={handleNextMeso} className="p-2 rounded-md active:bg-zinc-800 text-white active:scale-90 transition-all"><ChevronRight size={16} /></button>
          </div>
        </div>
        <div className="flex items-center gap-3 overflow-x-auto pb-2 no-scrollbar px-1" ref={weekScrollRef}>
          {weeksInMeso.map(w => {
            const isActive = currentWeek === w;
            const sessionsCount = Object.keys(plan).filter(k => k.startsWith(`${w}-`)).length;
            return (
              <button key={w} onClick={() => setCurrentWeek(w)} className={`flex-shrink-0 w-[95px] p-4 rounded-2xl border transition-all duration-300 active:scale-95 flex flex-col items-center gap-1 ${isActive ? 'bg-[#06B6D4] border-[#22D3EE] text-black shadow-lg shadow-cyan-950/20' : 'bg-[#141417] border-[#27272A] text-[#71717A]'}`}>
                <span className={`text-[8px] font-black uppercase tracking-widest ${isActive ? 'text-cyan-900' : 'text-[#52525B]'}`}>Semana</span>
                <span className={`text-2xl font-black ${isActive ? 'text-black' : 'text-white'}`}>{w}</span>
                <div className="flex gap-1 mt-1">
                  {[...Array(7)].map((_, i) => (
                    <div key={i} className={`w-1.5 h-1.5 rounded-full ${i < sessionsCount ? (isActive ? 'bg-cyan-900' : 'bg-[#06B6D4]') : (isActive ? 'bg-cyan-700/50' : 'bg-[#27272A]')}`} />
                  ))}
                </div>
              </button>
            );
          })}
        </div>
      </div>
    );
  };

  return (
    <div className={`min-h-screen ${s.bg} ${s.textSec} transition-all duration-300 font-sans`}>
      {/* Smart Banner Instalación iOS/Android */}
      {showInstallBanner && activeTab !== 'editor' && (
        <div className="bg-[#06B6D4]/10 border-b border-[#06B6D4]/20 py-2.5 px-4 flex items-center justify-between pt-[max(0.5rem,env(safe-area-inset-top))]">
          <div className="flex items-center gap-2">
            <Smartphone size={14} className="text-[#06B6D4]" />
            <span className="text-[11px] text-[#06B6D4] font-bold tracking-tight">Añade esta app a tu pantalla de inicio</span>
          </div>
          <button onClick={() => setShowInstallBanner(false)} className="p-1 active:bg-cyan-500/20 rounded-md"><X size={14} className="text-[#06B6D4]"/></button>
        </div>
      )}

      {/* Header Nativo (Respeta Safe Area) */}
      <header className={`sticky top-0 z-40 backdrop-blur-xl border-b ${s.header} px-6 ${showInstallBanner ? 'py-4' : 'pt-[max(1rem,env(safe-area-inset-top))] pb-4'}`}>
        <div className="max-w-xl mx-auto flex justify-between items-center">
          <div className="flex items-center gap-3">
            <div className="w-9 h-9 bg-[#06B6D4] rounded-xl flex items-center justify-center shadow-lg shadow-cyan-500/20"><Zap size={20} className="text-black fill-current" /></div>
            <h1 className={`text-xl font-black tracking-tighter ${s.textMain}`}>SPRINT <span className="text-[#06B6D4]">&</span> RELAX</h1>
          </div>
        </div>
      </header>

      {/* Contenido Principal con Padding para Bottom Navigation */}
      <main className="max-w-xl mx-auto px-5 mt-6 pb-32">
        {activeTab === 'calendario' ? (
          <div className="space-y-4 animate-in fade-in duration-500">
            <WeekSelector />
            <div className="flex items-center gap-2 mb-3 px-1 mt-6">
              <Activity size={12} className="text-[#06B6D4]" />
              <span className="text-[10px] font-black uppercase text-[#71717A] tracking-[0.2em]">Sesiones diarias</span>
            </div>
            {days.map(day => {
              const session = plan[`${currentWeek}-${day}`];
              const isRest = session?.type === 'descanso';
              return (
                <div key={day} onClick={() => openEditor(currentWeek, day)} className={`group active:scale-[0.98] cursor-pointer p-4 rounded-2xl border transition-transform duration-200 ${session ? (isRest ? 'bg-[#18181B]/40 border-[#27272A]' : 'bg-[#06B6D4]/5 border-[#06B6D4]/40') : `${s.card} opacity-80 active:opacity-100`}`}>
                  <div className="flex justify-between items-center">
                    <div className="flex items-center gap-4">
                      <div className={`w-12 h-12 rounded-[14px] flex items-center justify-center ${session ? (isRest ? 'bg-[#27272A] text-[#71717A]' : 'bg-[#06B6D4] text-black shadow-md shadow-cyan-500/20') : 'bg-[#1F1F23] text-[#52525B]'}`}>
                        {session?.type === 'carrera' ? <Zap size={22}/> : session?.type === 'fuerza' ? <Dumbbell size={22}/> : session?.type === 'natacion' ? <Waves size={22}/> : session?.type === 'descanso' ? <Moon size={22}/> : <Timer size={22}/>}
                      </div>
                      <div>
                        <span className="text-[10px] font-black uppercase text-[#71717A] tracking-widest">{day}</span>
                        <h3 className={`text-[15px] font-bold mt-0.5 leading-tight ${session ? (isRest ? 'text-[#71717A] italic' : 'text-[#F4F4F5]') : 'text-[#52525B]'}`}>{session ? session.name : 'Configurar sesión'}</h3>
                      </div>
                    </div>
                    {session ? <CheckCircle2 size={20} className={isRest ? "text-[#3F3F46]" : "text-[#06B6D4]"} /> : <Plus size={20} className="text-[#3F3F46]" />}
                  </div>
                </div>
              );
            })}
          </div>
        ) : activeTab === 'editor' ? (
          <div className="space-y-6 animate-in slide-in-from-right-4 duration-300 pb-32">
            <div className="flex items-center justify-between mb-2">
              <button onClick={() => { setEditingDay(null); setActiveTab('calendario'); }} className="flex items-center gap-2 p-2 -ml-2 text-[#A1A1AA] text-[11px] font-black uppercase tracking-widest active:text-[#06B6D4] active:bg-[#18181B] rounded-lg transition-all"><ArrowLeft size={18}/> Volver</button>
              <div className="text-right">
                <span className="text-[9px] font-black text-[#52525B] uppercase block tracking-widest mb-0.5">Meso {currentMesocycle}</span>
                <span className="text-[10px] font-black text-[#06B6D4] uppercase tracking-tighter">{editingDay?.day} · SEMANA {editingDay?.week}</span>
              </div>
            </div>

            <div className="grid grid-cols-5 gap-1 p-1.5 bg-[#18181B] rounded-[18px] border border-[#27272A]">
              {[
                { id: 'carrera', icon: <Zap size={18}/> },
                { id: 'fuerza', icon: <Dumbbell size={18}/> },
                { id: 'natacion', icon: <Waves size={18}/> },
                { id: 'cuerda', icon: <Timer size={18}/> },
                { id: 'descanso', icon: <Moon size={18}/> }
              ].map(type => (
                <button key={type.id} onClick={() => setSessionType(type.id)} className={`flex flex-col items-center gap-1.5 py-3 rounded-[14px] text-[9px] font-black uppercase transition-all active:scale-95 ${sessionType === type.id ? 'bg-[#06B6D4] text-black shadow-md' : 'text-[#52525B]'}`}>
                  {type.icon}
                  {type.id === 'descanso' ? 'Desc' : type.id}
                </button>
              ))}
            </div>

            {sessionType !== 'descanso' ? (
              <>
                <input type="text" placeholder="Título de la sesión" className="w-full bg-transparent border-none text-3xl font-black text-white placeholder-[#3F3F46] focus:ring-0 p-0 tracking-tight" value={sessionName} onChange={e => setSessionName(e.target.value)} />

                <div className="space-y-5">
                  {sessionType === 'carrera' && (
                    <div className="space-y-4">
                      <div className="flex gap-2">
                        {['rodaje', 'fartlek', 'series'].map(st => (
                          <button key={st} onClick={() => setIntervals([...intervals, { id: Math.random(), subType: st, notas: '', distancia: st === 'series' ? '400' : '10', ritmo: '4:30', reps: '10', tiempoFuerte: '1:00', tiempoSuave: '1:00', vamPct: '100', descanso: '1:30' }])} className="flex-1 py-3.5 rounded-[14px] bg-[#06B6D4]/10 border border-[#06B6D4]/20 text-[10px] font-black uppercase text-[#06B6D4] active:bg-[#06B6D4] active:text-black transition-all">+ {st}</button>
                        ))}
                      </div>
                      {intervals.map(int => (
                        <div key={int.id} className="p-5 rounded-[20px] bg-[#141417] border border-[#27272A] relative">
                          <button onClick={() => deleteBlock(int.id, 'carrera')} className="absolute top-5 right-5 p-2 -mr-2 -mt-2 text-[#52525B] active:text-red-500 active:bg-red-500/10 rounded-lg transition-all"><Trash2 size={18}/></button>
                          <span className="text-[10px] font-black text-[#06B6D4] uppercase tracking-[0.2em] block mb-5">{int.subType}</span>
                          <div className="grid grid-cols-3 gap-3 mb-4">
                            {int.subType === 'rodaje' ? (
                              <>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">KM</label><input inputMode="decimal" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base font-bold text-white" value={int.distancia} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, distancia: e.target.value} : i))}/></div>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">Ritmo</label><input inputMode="text" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base font-bold text-white" value={int.ritmo} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, ritmo: e.target.value} : i))}/></div>
                              </>
                            ) : int.subType === 'fartlek' ? (
                              <>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">Reps</label><input inputMode="numeric" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base font-bold text-white" value={int.reps} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, reps: e.target.value} : i))}/></div>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">F+S</label><input inputMode="text" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base font-bold text-white" value={`${int.tiempoFuerte}+${int.tiempoSuave}`} onChange={e => {
                                  const [f, s] = e.target.value.split('+');
                                  setIntervals(intervals.map(i => i.id === int.id ? {...i, tiempoFuerte: f || '', tiempoSuave: s || ''} : i));
                                }}/></div>
                              </>
                            ) : (
                              <>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">Series x M</label><div className="flex items-center gap-1.5"><input inputMode="numeric" className="w-12 bg-[#1F1F23] border-none rounded-[14px] p-3 text-base font-bold text-center text-white px-1" value={int.reps} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, reps: e.target.value} : i))}/><span className="text-[10px] text-[#71717A] font-black">x</span><input inputMode="numeric" className="w-16 bg-[#1F1F23] border-none rounded-[14px] p-3 text-base font-bold text-center text-white px-1" value={int.distancia} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, distancia: e.target.value} : i))}/></div></div>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">% VAM</label><input inputMode="numeric" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base text-[#06B6D4] font-black" value={int.vamPct} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, vamPct: e.target.value} : i))}/></div>
                                <div className="space-y-1.5"><label className="text-[8px] font-black uppercase text-[#71717A] tracking-widest">Rec</label><input inputMode="text" className="w-full bg-[#1F1F23] border-none rounded-[14px] p-3.5 text-base font-bold text-white" value={int.descanso} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, descanso: e.target.value} : i))}/></div>
                              </>
                            )}
                          </div>
                          <div className="space-y-1.5 bg-[#0A0A0C]/60 p-3.5 rounded-[16px] border border-[#27272A]">
                            <label className="text-[8px] font-black uppercase text-[#71717A] flex items-center gap-1.5 mb-1"><StickyNote size={12} className="text-[#06B6D4]"/> Notas</label>
                            <textarea className="w-full bg-transparent text-sm font-medium resize-none focus:outline-none text-[#D4D4D8]" rows="2" value={int.notas} onChange={e => setIntervals(intervals.map(i => i.id === int.id ? {...i, notas: e.target.value} : i))} />
                          </div>
                        </div>
                      ))}
                    </div>
                  )}

                  {(sessionType === 'fuerza' || sessionType === 'cuerda' || sessionType === 'natacion') && (
                    <div className="space-y-6">
                      <button onClick={() => {
                        const newBlock = { id: Math.random(), descansoSerie: sessionType === 'natacion' ? '0:20' : '1:30', items: [{ id: Math.random(), nombre: '', series: '3', reps: sessionType === 'natacion' ? '100m' : '12', rpe: '7', ritmo: sessionType === 'natacion' ? '1:45' : '', notas: '' }] };
                        const setter = sessionType === 'fuerza' ? setExercises : sessionType === 'natacion' ? setSwimBlocks : setRopeBlocks;
                        const list = sessionType === 'fuerza' ? exercises : sessionType === 'natacion' ? swimBlocks : ropeBlocks;
                        setter([...list, newBlock]);
                      }} className={`w-full py-5 border-2 border-dashed border-[#27272A] rounded-[20px] text-[10px] font-black uppercase text-[#71717A] active:bg-[#18181B] active:text-[#06B6D4] active:border-[#06B6D4]/30 transition-all`}>
                        + Añadir Bloque {sessionType}
                      </button>
                      
                      {(sessionType === 'fuerza' ? exercises : sessionType === 'natacion' ? swimBlocks : ropeBlocks).map((block, bIdx) => (
                        <div key={block.id} className={`p-5 rounded-[24px] border space-y-4 bg-[#141417] border-[#27272A] shadow-sm`}>
                          <div className="flex justify-between items-center mb-1">
                            <span className={`text-[10px] font-black uppercase tracking-[0.2em] text-[#06B6D4]`}>BLOQUE {bIdx+1}</span>
                            <div className="flex gap-3 items-center">
                              <div className="flex items-center gap-1.5 px-3 py-2 rounded-full bg-[#0A0A0C]">
                                <Clock size={12} className="text-[#71717A]"/>
                                <input inputMode="text" className="bg-transparent border-none p-0 text-[11px] w-12 font-black text-center text-white" value={block.descansoSerie} onChange={e => {
                                  const setter = sessionType === 'fuerza' ? setExercises : sessionType === 'natacion' ? setSwimBlocks : setRopeBlocks;
                                  const list = sessionType === 'fuerza' ? exercises : sessionType === 'natacion' ? swimBlocks : ropeBlocks;
                                  setter(list.map(b => b.id === block.id ? {...b, descansoSerie: e.target.value} : b));
                                }}/>
                              </div>
                              <button onClick={() => deleteBlock(block.id, sessionType)} className="p-2 -mr-2 text-[#52525B] active:text-red-500 active:bg-red-500/10 rounded-full transition-all"><Trash2 size={18} /></button>
                            </div>
                          </div>

                          {block.items.map((item, iIdx) => (
                            <div key={item.id} className="bg-[#1F1F23]/80 p-4 rounded-[18px] border border-[#27272A] relative">
                              <button onClick={() => {
                                 const setter = sessionType === 'fuerza' ? setExercises : sessionType === 'natacion' ? setSwimBlocks : setRopeBlocks;
                                 const list = sessionType === 'fuerza' ? exercises : sessionType === 'natacion' ? swimBlocks : ropeBlocks;
                                 setter(list.map(b => b.id === block.id ? {...b, items: b.items.filter(e => e.id !== item.id)} : b));
                              }} className="absolute top-3 right-3 p-2 -mt-1 -mr-1 text-[#71717A] active:bg-[#27272A] rounded-full transition-all"><X size={16} /></button>
                              
                              <input placeholder={sessionType === 'natacion' ? "Ejercicio de nado" : "Nombre del ejercicio"} className="w-full bg-transparent text-[15px] font-black border-none outline-none mb-4 text-white placeholder-[#52525B] tracking-tight pr-8" value={item.nombre} onChange={e => updateItemField(block.id, item.id, 'nombre', e.target.value, sessionType)}/>

                              <div className="grid grid-cols-3 gap-2.5">
                                 <div className="bg-[#0A0A0C] p-2.5 rounded-[12px] text-center"><label className="text-[8px] uppercase font-black text-[#71717A] block mb-1">Series</label><input inputMode="numeric" className="bg-transparent text-center w-full text-sm font-bold text-white" value={item.series} onChange={e => updateItemField(block.id, item.id, 'series', e.target.value, sessionType)}/></div>
                                 <div className="bg-[#0A0A0C] p-2.5 rounded-[12px] text-center"><label className="text-[8px] uppercase font-black text-[#71717A] block mb-1">{sessionType === 'natacion' ? 'Metros' : 'Reps'}</label><input inputMode="text" className="bg-transparent text-center w-full text-sm font-bold text-white" value={item.reps} onChange={e => updateItemField(block.id, item.id, 'reps', e.target.value, sessionType)}/></div>
                                 {sessionType === 'natacion' ? (
                                   <div className="bg-[#0A0A0C] p-2.5 rounded-[12px] text-center border border-cyan-500/20"><label className="text-[8px] uppercase font-black text-[#06B6D4] block mb-1">Ritmo</label><input inputMode="text" className="bg-transparent text-center w-full text-sm font-black text-[#06B6D4]" value={item.ritmo} onChange={e => updateItemField(block.id, item.id, 'ritmo', e.target.value, sessionType)}/></div>
                                 ) : (
                                   <div className="bg-[#0A0A0C] p-2.5 rounded-[12px] text-center border border-orange-500/20"><label className="text-[8px] uppercase font-black text-orange-500 block mb-1">RPE</label><input inputMode="numeric" className="bg-transparent text-center w-full text-sm font-black text-orange-500" value={item.rpe} onChange={e => updateItemField(block.id, item.id, 'rpe', e.target.value, sessionType)}/></div>
                                 )}
                              </div>
                              <textarea placeholder="Notas adicionales..." className="w-full bg-[#0A0A0C]/60 p-3 rounded-[12px] text-xs mt-3 resize-none outline-none focus:bg-[#0A0A0C] text-[#A1A1AA]" rows="1" value={item.notas} onChange={e => updateItemField(block.id, item.id, 'notas', e.target.value, sessionType)} />
                            </div>
                          ))}
                          <button onClick={() => {
                            const setter = sessionType === 'fuerza' ? setExercises : sessionType === 'natacion' ? setSwimBlocks : setRopeBlocks;
                            const list = sessionType === 'fuerza' ? exercises : sessionType === 'natacion' ? swimBlocks : ropeBlocks;
                            setter(list.map(b => b.id === block.id ? { ...b, items: [...b.items, { id: Math.random(), nombre: '', series: '3', reps: sessionType === 'natacion' ? '100m' : '12', rpe: '7', ritmo: sessionType === 'natacion' ? '1:45' : '', notas: '' }] } : b));
                          }} className={`w-full py-3.5 bg-[#1F1F23] active:bg-[#27272A] rounded-[16px] text-[10px] font-black uppercase text-[#A1A1AA] transition-colors`}>+ Añadir Línea</button>
                        </div>
                      ))}
                    </div>
                  )}
                </div>
              </>
            ) : (
              <div className="flex flex-col items-center justify-center py-20 bg-[#141417] rounded-[24px] border border-[#27272A] border-dashed mt-10">
                <Moon size={56} className="text-[#27272A] mb-5" />
                <h3 className="text-xl font-black text-[#71717A] uppercase tracking-tighter">Día de recuperación</h3>
                <p className="text-[10px] font-bold text-[#52525B] uppercase mt-3 tracking-[0.3em]">Cargando pilas para mañana</p>
              </div>
            )}

            {/* Editor Bottom Action Bar (Safe Area) */}
            <div className="fixed bottom-0 left-0 right-0 px-6 pt-6 pb-[max(2rem,env(safe-area-inset-bottom))] bg-gradient-to-t from-[#0A0A0C] via-[#0A0A0C]/95 to-transparent pointer-events-none z-50">
               <button onClick={saveAndReturn} className="max-w-xl mx-auto w-full bg-[#06B6D4] py-4.5 rounded-[18px] font-black text-[12px] uppercase tracking-[0.3em] text-black shadow-2xl shadow-cyan-500/30 pointer-events-auto flex items-center justify-center gap-3 active:bg-[#22D3EE] active:scale-[0.98] transition-all h-[60px]">
                 <Save size={20}/> {sessionType === 'descanso' ? 'Confirmar Descanso' : 'Guardar Sesión'}
               </button>
            </div>
          </div>
        ) : (
          <div className="space-y-6 animate-in fade-in duration-500 pb-10">
            <WeekSelector />
            <div className="text-center pt-2 pb-4">
              <h2 className="text-2xl font-black text-white uppercase tracking-tighter">Resumen Semanal</h2>
              <div className="h-1.5 w-12 bg-[#06B6D4] mx-auto mt-3 rounded-full shadow-lg shadow-cyan-500/20"></div>
            </div>
            <div className="space-y-6">
              {days.map(day => {
                const session = plan[`${currentWeek}-${day}`];
                const isRest = session?.type === 'descanso';
                const theme = getSessionTheme(session?.type);
                
                if (!session || isRest) return (
                   <div key={day} className="flex justify-between items-center py-5 border-b border-[#1F1F23]">
                     <div className="flex items-center gap-3 opacity-50">
                        <Moon size={18} className="text-[#52525B]" />
                        <span className="text-[11px] font-black uppercase tracking-[0.2em]">{day}</span>
                     </div>
                     <span className="text-[10px] font-bold italic uppercase tracking-[0.3em] text-[#52525B]">Descanso</span>
                   </div>
                );

                return (
                  <div key={day} className="space-y-4 pb-8 border-b border-[#1F1F23]">
                    <div className="flex justify-between items-end">
                      <div className="flex items-center gap-3">
                         <div className={`p-2.5 rounded-[12px] ${theme.iconBg} ${theme.accent}`}>
                           {theme.icon}
                         </div>
                         <span className={`text-[11px] font-black uppercase tracking-[0.2em] ${theme.accent}`}>{day}</span>
                      </div>
                      <span className="text-[15px] font-black text-white uppercase tracking-tight max-w-[60%] text-right leading-tight">{session.name}</span>
                    </div>
                    
                    <div className="grid grid-cols-1 gap-3">
                       {session.data.map((block, bIdx) => (
                         <div key={bIdx} className={`p-5 rounded-[20px] border ${theme.bg} ${theme.border} space-y-4`}>
                            {session.type === 'carrera' && (
                              <div className="space-y-3.5">
                                <div className="flex justify-between text-[10px] font-black uppercase text-[#71717A] tracking-widest border-b border-white/5 pb-2.5">
                                   <span>BLOQUE {bIdx+1}</span>
                                   <span className={theme.accent}>Rec: {block.subType === 'series' ? block.descanso : (block.subType === 'fartlek' ? 'N/A' : 'S/R')}</span>
                                </div>
                                <div className="flex justify-between items-center">
                                   <span className="text-[#F4F4F5] font-black uppercase tracking-[0.15em] text-[11px]">
                                      {block.subType}
                                   </span>
                                   <span className={`font-black uppercase tracking-tighter text-[15px] text-right ${theme.accent}`}>
                                      {block.subType === 'rodaje' ? `${block.distancia}km @ ${block.ritmo}` : 
                                       block.subType === 'series' ? `${block.reps}x${block.distancia}m @ ${block.vamPct}% VAM` : 
                                       `${block.reps} reps (${block.tiempoFuerte}/${block.tiempoSuave})`}
                                   </span>
                                </div>
                                {block.notas && <p className="text-[11px] text-[#A1A1AA] italic border-l-[3px] border-white/10 pl-3 py-0.5 leading-relaxed">"{block.notas}"</p>}
                              </div>
                            )}

                            {(session.type === 'fuerza' || session.type === 'cuerda' || session.type === 'natacion') && (
                              <div className="space-y-3.5">
                                <div className={`flex justify-between text-[10px] font-black uppercase text-[#71717A] tracking-widest border-b border-white/5 pb-2.5`}>
                                   <span>BLOQUE {bIdx+1}</span>
                                   <span className={theme.accent}>Rec: {block.descansoSerie}</span>
                                </div>
                                {block.items.map((item, eIdx) => (
                                  <div key={eIdx} className="space-y-1.5">
                                    <div className="flex justify-between items-start gap-3">
                                      <span className="text-[#F4F4F5] font-bold uppercase tracking-tight text-[12px] leading-tight flex-1">{item.nombre || (session.type === 'natacion' ? 'Nado' : 'Ejercicio')}</span>
                                      <span className={`font-black uppercase tracking-widest text-[12px] whitespace-nowrap ${theme.accent}`}>
                                        {item.series}x{item.reps} 
                                        {session.type === 'natacion' ? <span className="ml-1.5 text-white/50">@{item.ritmo}</span> : (item.rpe && <span className="text-orange-500 ml-1.5">RPE {item.rpe}</span>)}
                                      </span>
                                    </div>
                                    {item.notas && <p className="text-[11px] text-[#A1A1AA] italic pl-3 border-l-[3px] border-white/10 py-0.5">"{item.notas}"</p>}
                                  </div>
                                ))}
                              </div>
                            )}
                         </div>
                       ))}
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        )}
      </main>

      {/* Tab Bar Nativo Inferior para Móviles */}
      {activeTab !== 'editor' && (
        <nav className="fixed bottom-0 w-full bg-[#141417]/95 backdrop-blur-xl border-t border-[#27272A] pb-[max(0.8rem,env(safe-area-inset-bottom))] pt-2.5 px-6 z-50 flex justify-around shadow-[0_-10px_40px_rgba(0,0,0,0.5)]">
          <button onClick={() => setActiveTab('calendario')} className={`flex flex-col items-center gap-1.5 transition-all p-2 w-20 active:scale-95 ${activeTab === 'calendario' ? 'text-[#06B6D4]' : 'text-[#71717A]'}`}>
            <Calendar size={24} className={activeTab === 'calendario' ? 'fill-cyan-500/10' : ''} />
            <span className="text-[9px] font-black uppercase tracking-widest">Plan</span>
          </button>
          <button onClick={() => setActiveTab('resumen')} className={`flex flex-col items-center gap-1.5 transition-all p-2 w-20 active:scale-95 ${activeTab === 'resumen' ? 'text-[#06B6D4]' : 'text-[#71717A]'}`}>
            <FileText size={24} className={activeTab === 'resumen' ? 'fill-cyan-500/10' : ''} />
            <span className="text-[9px] font-black uppercase tracking-widest">Resumen</span>
          </button>
        </nav>
      )}
      
      <style>{`
        /* CSS para comportamiento 100% nativo (PWA App) */
        :root {
          color-scheme: dark;
        }
        body { 
          font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; 
          -webkit-tap-highlight-color: transparent; /* Evita parpadeos azules al tocar en Android/iOS */
          touch-action: manipulation; /* Optimiza retraso de 300ms */
          user-select: none; /* Sensación nativa: texto no seleccionable globalmente */
          -webkit-user-select: none;
        }
        input, textarea {
          user-select: auto; /* Permite selección solo en inputs */
          -webkit-user-select: auto;
          font-size: 16px !important; /* Previene el Auto-Zoom en inputs de iOS Safari */
        }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
      `}</style>
    </div>
  );
};

export default App;
