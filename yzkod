import React, { useState } from 'react';
import { GRADES, SUBJECTS } from '../constants';
import { generateLessonDetails } from '../services/geminiService';
import { Loader2, BookOpen, Clock, Users, Target, FileText, ListChecks, Puzzle, Brain } from 'lucide-react';
import ReactMarkdown from 'react-markdown';

type ContentType = 'plan' | 'quiz' | 'notes' | 'activity';

const LessonGenerator: React.FC = () => {
  const [grade, setGrade] = useState<string>(GRADES[0]);
  const [subject, setSubject] = useState<string>(SUBJECTS[0]);
  const [topic, setTopic] = useState('');
  const [contentType, setContentType] = useState<ContentType>('plan');
  const [content, setContent] = useState('');
  const [loading, setLoading] = useState(false);

  const handleGenerate = async () => {
    if (!topic) return;
    setLoading(true);
    const result = await generateLessonDetails(grade, subject, topic, contentType);
    setContent(result);
    setLoading(false);
  };

  const contentTypes = [
    { id: 'plan', label: 'Ders Planı', icon: Clock, desc: '5E Modeli, 40dk akış' },
    { id: 'quiz', label: 'Sınav Sorusu', icon: ListChecks, desc: '10 soru + Cevaplar' },
    { id: 'notes', label: 'Ders Notu', icon: FileText, desc: 'Özet ve kavramlar' },
    { id: 'activity', label: 'Etkinlik', icon: Puzzle, desc: 'Sınıf içi oyunlar' },
  ];

  return (
    <div className="grid grid-cols-1 lg:grid-cols-3 gap-6 lg:h-[calc(100vh-8rem)]">
      {/* Input Panel */}
      <div className="lg:col-span-1 bg-slate-900 p-6 rounded-2xl shadow-sm border border-slate-800 flex flex-col h-auto lg:h-full overflow-y-auto custom-scrollbar">
        <h2 className="text-xl font-bold text-blue-400 mb-6 flex items-center space-x-2">
          <BookOpen className="text-blue-500" size={24} />
          <span>İçerik Stüdyosu</span>
        </h2>
        
        <div className="space-y-6 flex-1">
          {/* Content Type Selector */}
          <div>
            <label className="block text-xs font-bold text-slate-500 uppercase mb-3">Ne Üretmek İstersin?</label>
            <div className="grid grid-cols-2 gap-3">
              {contentTypes.map((type) => {
                const Icon = type.icon;
                const isSelected = contentType === type.id;
                return (
                  <button
                    key={type.id}
                    onClick={() => setContentType(type.id as ContentType)}
                    className={`p-3 rounded-xl border text-left transition-all ${
                      isSelected 
                        ? 'bg-blue-600 border-blue-500 text-white shadow-lg shadow-blue-900/20' 
                        : 'bg-slate-950 border-slate-800 text-slate-400 hover:border-slate-600 hover:bg-slate-900'
                    }`}
                  >
                    <Icon size={20} className={`mb-2 ${isSelected ? 'text-white' : 'text-blue-500'}`} />
                    <div className="font-bold text-sm">{type.label}</div>
                    <div className={`text-[10px] ${isSelected ? 'text-blue-100' : 'text-slate-600'}`}>{type.desc}</div>
                  </button>
                );
              })}
            </div>
          </div>

          <div className="space-y-4">
            <div>
              <label className="block text-xs font-bold text-slate-500 uppercase mb-2">Sınıf</label>
              <select 
                value={grade} onChange={(e) => setGrade(e.target.value)}
                className="w-full p-3 border border-slate-700 rounded-xl bg-slate-950 text-white font-medium appearance-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none transition-all"
              >
                {GRADES.map(g => <option key={g} value={g}>{g}</option>)}
              </select>
            </div>
            
            <div>
              <label className="block text-xs font-bold text-slate-500 uppercase mb-2">Ders</label>
              <select 
                value={subject} onChange={(e) => setSubject(e.target.value)}
                className="w-full p-3 border border-slate-700 rounded-xl bg-slate-950 text-white font-medium appearance-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 outline-none transition-all"
              >
                {SUBJECTS.map(s => <option key={s} value={s}>{s}</option>)}
              </select>
            </div>

            <div>
              <label className="block text-xs font-bold text-slate-500 uppercase mb-2">Konu Başlığı</label>
              <input 
                type="text" 
                value={topic}
                onChange={(e) => setTopic(e.target.value)}
                placeholder="Örn: Fotosentez, Türev, I. Dünya Savaşı..."
                className="w-full p-3 border border-slate-700 rounded-xl bg-slate-950 focus:ring-2 focus:ring-blue-500 text-white focus:border-blue-500 outline-none placeholder:text-slate-600 transition-all"
              />
            </div>
          </div>

          <div className="bg-blue-900/10 p-4 rounded-xl border border-blue-900/30">
            <h4 className="text-xs font-bold text-blue-400 mb-2 uppercase tracking-wider flex items-center">
              <Brain size={14} className="mr-2"/> AI Modülü Hazır
            </h4>
            <p className="text-xs text-slate-400 leading-relaxed">
              Seçilen parametrelere göre <span className="text-blue-300 font-bold">{grade}</span> seviyesine uygun, özgün bir <span className="text-blue-300 font-bold">{contentTypes.find(t => t.id === contentType)?.label}</span> oluşturulacak.
            </p>
          </div>
        </div>

        <button
          onClick={handleGenerate}
          disabled={loading || !topic}
          className="mt-6 w-full bg-blue-600 hover:bg-blue-500 text-white font-bold py-4 px-4 rounded-xl transition-all shadow-lg shadow-blue-900/20 flex items-center justify-center space-x-2 disabled:opacity-50 hover:scale-[1.02] active:scale-[0.98]"
        >
          {loading ? <Loader2 className="animate-spin" /> : <span>Oluştur (AI)</span>}
        </button>
      </div>

      {/* Output Panel */}
      <div className="lg:col-span-2 bg-slate-900 rounded-2xl shadow-sm border border-slate-800 flex flex-col h-[600px] lg:h-full overflow-hidden">
        <div className="p-4 border-b border-slate-800 bg-slate-950 font-bold text-blue-400 flex justify-between items-center">
          <span className="flex items-center space-x-2">
            {contentType === 'quiz' && <ListChecks size={18}/>}
            {contentType === 'plan' && <Clock size={18}/>}
            {contentType === 'notes' && <FileText size={18}/>}
            {contentType === 'activity' && <Puzzle size={18}/>}
            <span>Üretilen İçerik</span>
          </span>
          <span className="text-[10px] bg-blue-900/30 text-blue-300 px-2 py-1 rounded border border-blue-800 font-mono">MARKDOWN</span>
        </div>
        <div className="flex-1 p-8 overflow-y-auto custom-scrollbar bg-slate-900/50">
          {content ? (
            <ReactMarkdown className="prose prose-invert max-w-none prose-headings:text-white prose-p:text-slate-300 prose-li:text-slate-300 prose-strong:text-blue-400 prose-a:text-blue-400 prose-blockquote:border-blue-500 prose-blockquote:bg-blue-900/10 prose-blockquote:p-4 prose-blockquote:rounded-r-lg prose-code:text-blue-300">
                {content}
            </ReactMarkdown>
          ) : (
            <div className="flex flex-col items-center justify-center h-full text-slate-600">
              <div className="w-24 h-24 bg-slate-800 rounded-full flex items-center justify-center mb-6 animate-pulse">
                 <BookOpen size={40} className="text-slate-700" />
              </div>
              <p className="font-bold text-lg text-slate-500">Henüz içerik oluşturulmadı.</p>
              <p className="text-sm mt-2 opacity-50 max-w-xs text-center">Sol panelden içerik türünü seç, konuyu gir ve yapay zekanın sihrini izle.</p>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default LessonGenerator;

