import React, { useEffect, useState } from 'react';

const LINKS = [
  { label: 'Twitter', tag: 'X', url: 'https://twitter.com/yourhandle' },
  { label: 'GitHub', tag: 'GH', url: 'https://github.com/yourhandle' },
  { label: 'Portfolio', tag: 'WWW', url: 'https://yourportfolio.dev' },
  { label: 'Email', tag: '@', url: 'mailto:you@example.com' },
];

const PROFILE = {
  name: 'Your Name',
  handle: '@yourhandle',
  bio: 'Building things on the internet, one commit at a time.',
};

export default function App() {
  const [isDark, setIsDark] = useState(true);
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    // Trigger the staggered entrance animation shortly after first paint
    const t = setTimeout(() => setMounted(true), 60);
    return () => clearTimeout(t);
  }, []);

  const bg = isDark ? 'bg-void' : 'bg-ink';
  const text = isDark ? 'text-ink' : 'text-void';
  const panel = isDark ? 'bg-panel border-white/10' : 'bg-white border-black/10';
  const muted = 'text-muted';

  return (
    <div className={`min-h-screen ${bg} ${text} font-body flex items-center justify-center px-5 py-16 transition-colors duration-500`}>
      <div className="w-full max-w-sm">

        {/* Theme toggle */}
        <div
          className={`flex justify-end mb-8 transition-all duration-500 ${mounted ? 'opacity-100' : 'opacity-0 -translate-y-2'}`}
        >
          <button
            onClick={() => setIsDark(!isDark)}
            aria-label="Toggle dark and light mode"
            className={`font-mono text-xs px-3 py-1.5 rounded-full border ${panel} ${muted} hover:text-signal transition-colors`}
          >
            {isDark ? '○ light' : '● dark'}
          </button>
        </div>

        {/* Profile */}
        <div
          className={`mb-10 transition-all duration-500 ${mounted ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-3'}`}
        >
          <p className={`font-mono text-xs ${muted} mb-3`}>{PROFILE.handle}</p>
          <h1 className="font-display text-3xl font-bold mb-3 leading-tight">
            {PROFILE.name}
            <span className="inline-block w-[2px] h-6 bg-signal ml-1 align-middle animate-pulse" />
          </h1>
          <p className={`${muted} text-sm leading-relaxed`}>{PROFILE.bio}</p>
        </div>

        {/* Links */}
        <div className="flex flex-col gap-3">
          {LINKS.map((link, i) => (
            <a
              key={link.label}
              href={link.url}
              target="_blank"
              rel="noopener noreferrer"
              className={`group flex items-center justify-between rounded-xl border ${panel} px-4 py-3.5
                transition-all duration-500 hover:border-signal/60
                ${mounted ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-3'}`}
              style={{ transitionDelay: mounted ? `${120 + i * 80}ms` : '0ms' }}
            >
              <span className="flex items-center gap-3">
                <span className="font-mono text-[11px] px-2 py-1 rounded-md bg-signal/15 text-signal">
                  {link.tag}
                </span>
                <span className="text-sm font-medium">{link.label}</span>
              </span>
              <span className={`font-mono text-sm ${muted} transition-transform group-hover:translate-x-1 group-hover:text-signal`}>
                →
              </span>
            </a>
          ))}
        </div>
    </div>
  );
}
