import React, { useState } from 'react';

/**
 * STANDARD EDITION
 * This version uses pure inline styles and no external libraries.
 * It is guaranteed to build on Vercel without errors.
 */

export default function App() {
  const [balance, setBalance] = useState({ sc: 5.00, playthroughReached: 0 });
  const [view, setView] = useState('game'); 
  const [isSpinning, setIsSpinning] = useState(false);
  const [reels, setReels] = useState(['🎰', '🎰', '🎰']);

  const spin = () => {
    if (isSpinning || balance.sc < 0.20) return;
    
    setIsSpinning(true);
    // Fast shuffle animation
    const interval = setInterval(() => {
      setReels([
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)], 
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)], 
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)]
      ]);
    }, 70);

    setTimeout(() => {
      clearInterval(interval);
      const res = [
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)], 
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)], 
        ['🍒','💎','🎰','🔔','🍋'][Math.floor(Math.random()*5)]
      ];
      setReels(res);
      
      const isWin = res[0] === res[1] && res[1] === res[2];
      const winAmount = isWin ? 2.00 : 0;
      
      setBalance(prev => ({
        sc: prev.sc - 0.20 + winAmount,
        playthroughReached: prev.playthroughReached + 0.20
      }));
      
      setIsSpinning(false);
    }, 800);
  };

  const progress = Math.min(100, (balance.playthroughReached / 15) * 100);

  return (
    <div style={{ minHeight: '100vh', backgroundColor: 'black', color: 'white', padding: '20px', fontFamily: 'system-ui, sans-serif', boxSizing: 'border-box' }}>
      {/* Header */}
      <header style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '30px' }}>
        <h1 style={{ color: '#EAB308', fontStyle: 'italic', fontWeight: '900', margin: 0, fontSize: '24px' }}>PROSLOTS</h1>
        <div style={{ backgroundColor: '#18181B', padding: '10px 18px', borderRadius: '12px', border: '1px solid #27272A', textAlign: 'right' }}>
          <div style={{ fontSize: '10px', color: '#71717A', fontWeight: 'bold', marginBottom: '2px' }}>SC BALANCE</div>
          <div style={{ color: '#22D3EE', fontWeight: '900', fontSize: '18px' }}>${balance.sc.toFixed(2)}</div>
        </div>
      </header>

      {/* Main Game Area */}
      {view === 'game' && (
        <div style={{ maxWidth: '400px', margin: '0 auto' }}>
          <div style={{ backgroundColor: '#18181B', borderRadius: '40px', padding: '40px 20px', textAlign: 'center', border: '1px solid #27272A', boxShadow: '0 20px 50px rgba(0,0,0,0.5)' }}>
             <div style={{ display: 'flex', justifyContent: 'center', gap: '10px', marginBottom: '40px', padding: '30px 0', backgroundColor: '#09090B', borderRadius: '24px', border: '2px solid #27272A' }}>
                {reels.map((s, i) => (
                  <div key={i} style={{ 
                    width: '70px', 
                    height: '100px', 
                    backgroundColor: 'white', 
                    borderRadius: '12px', 
                    display: 'flex', 
                    alignItems: 'center', 
                    justifyContent: 'center', 
                    fontSize: '40px', 
                    color: 'black', 
                    boxShadow: 'inset 0 0 10px rgba(0,0,0,0.2)' 
                  }}>
                    {s}
                  </div>
                ))}
             </div>
             
             <button 
               onClick={spin} 
               disabled={isSpinning || balance.sc < 0.20} 
               style={{ 
                 width: '100%', 
                 padding: '24px', 
                 backgroundColor: balance.sc < 0.20 ? '#3F3F46' : '#EAB308', 
                 color: 'black', 
                 borderRadius: '24px', 
                 fontWeight: '900', 
                 fontSize: '22px', 
                 border: 'none', 
                 cursor: 'pointer',
                 transition: 'transform 0.1s',
                 boxShadow: '0 8px 0 #A16207'
               }}
             >
                {isSpinning ? 'SPINNING...' : balance.sc < 0.20 ? 'ADD FUNDS' : 'SPIN $0.20'}
             </button>
          </div>
          
          {/* Progress Audit */}
          <div style={{ marginTop: '25px', backgroundColor: 'rgba(24,24,27,0.5)', padding: '20px', borderRadius: '24px', border: '1px solid #27272A' }}>
             <div style={{ display: 'flex', justifyContent: 'space-between', fontSize: '11px', fontWeight: '900', color: '#A1A1AA', marginBottom: '10px' }}>
                <span>PLAY AUDIT (UNLOCK STATUS)</span>
                <span style={{ color: '#EAB308' }}>{Math.round(progress)}%</span>
             </div>
             <div style={{ width: '100%', backgroundColor: 'black', height: '10px', borderRadius: '5px' }}>
                <div style={{ width: `${progress}%`, backgroundColor: '#EAB308', height: '100%', borderRadius: '5px', transition: 'width 0.5s ease-out' }}></div>
             </div>
             <p style={{ fontSize: '10px', color: '#52525B', marginTop: '10px', textAlign: 'center', lineHeight: '1.4' }}>
               Reach 100% and top up $10 to unlock instant cashouts to your CashApp.
             </p>
          </div>
        </div>
      )}

      {/* Navigation */}
      <nav style={{ 
        position: 'fixed', 
        bottom: '30px', 
        left: '20px', 
        right: '20px', 
        backgroundColor: 'rgba(24,24,27,0.95)', 
        backdropFilter: 'blur(10px)',
        borderRadius: '50px', 
        display: 'flex', 
        justifyContent: 'space-around', 
        padding: '12px', 
        maxWidth: '400px', 
        margin: '0 auto',
        border: '1px solid rgba(255,255,255,0.1)'
      }}>
        <button onClick={() => setView('game')} style={{ background: 'none', border: 'none', fontSize: '24px', opacity: view === 'game' ? 1 : 0.4, cursor: 'pointer' }}>⚡</button>
        <button onClick={() => setView('store')} style={{ background: 'none', border: 'none', fontSize: '24px', opacity: view === 'store' ? 1 : 0.4, cursor: 'pointer' }}>🛍️</button>
        <button onClick={() => setView('profile')} style={{ background: 'none', border: 'none', fontSize: '24px', opacity: view === 'profile' ? 1 : 0.4, cursor: 'pointer' }}>👤</button>
      </nav>
    </div>
  );
}

