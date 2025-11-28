import React, { useState, useEffect } from 'react';
import { Sprout, Trash2, Book, X } from 'lucide-react';

const TIER_COLORS = {
  basic: '#10b981',
  rare: '#3b82f6',
  epic: '#a855f7',
  extinct: '#ef4444'
};

const PLANT_ICONS = {
  // Basic tier
  'b1': ['🌱', '🪴', '🌿', '🍀', '☘️'],
  'b2': ['🌱', '🌼', '🌻', '🌸', '🏵️'],
  'b3': ['🌱', '🍀', '☘️', '🌿', '🪴'],
  'b4': ['🌱', '🌺', '🌸', '🌼', '💐'],
  'b5': ['🌱', '🌷', '🥀', '🌸', '💮'],
  
  // Rare tier
  'r1': ['🌱', '🪴', '🌸', '✨', '⭐'],
  'r2': ['🌱', '🌹', '🥀', '🔥', '💥'],
  'r3': ['🌱', '🌸', '💎', '🔮', '💠'],
  'r4': ['🌱', '🍃', '⚡', '🌩️', '⛈️'],
  'r5': ['🌱', '🌿', '🪴', '🧙', '✨'],
  
  // Epic tier
  'e1': ['🌱', '🔥', '🌋', '🦅', '🔆'],
  'e2': ['🌱', '🌸', '🌑', '🕳️', '⚫'],
  'e3': ['🌱', '🌷', '⏰', '🕰️', '⏳'],
  'e4': ['🌱', '🌺', '✨', '💫', '🌟'],
  'e5': ['🌱', '🌻', '💥', '☀️', '🔥'],
  
  // Extinct tier
  'x1': ['🌱', '🌸', '💠', '♾️', '🌌'],
  'x2': ['🌱', '🌺', '🪐', '🌠', '🌌'],
  'x3': ['🌱', '🌸', '🐉', '🗿', '👑'],
  'x4': ['🌱', '🌼', '✨', '👼', '😇'],
  'x5': ['🌱', '🌷', '🦕', '🌋', '🌍']
};

const PLANTS = {
  basic: [
    { id: 'b1', name: 'Moonwhisper Fern', description: 'A delicate fern that glows softly under moonlight, said to grant clarity of thought to those who tend it.' },
    { id: 'b2', name: 'Dawnpetal Daisy', description: 'Blooms at first light, filling the air with energy and motivation for the day ahead.' },
    { id: 'b3', name: 'Whisperleaf Clover', description: 'A lucky charm that whispers gentle encouragement during difficult tasks.' },
    { id: 'b4', name: 'Sunburst Poppy', description: 'Radiates warmth and positivity, banishing procrastination from your garden.' },
    { id: 'b5', name: 'Dewdrop Lily', description: 'Refreshes the mind like morning dew, perfect for starting new projects.' }
  ],
  rare: [
    { id: 'r1', name: 'Stardust Orchid', description: 'Blooms with cosmic energy, enhancing focus and turning distractions into stardust.' },
    { id: 'r2', name: 'Emberbloom Rose', description: 'Burns with quiet determination, keeping your motivation aflame through long tasks.' },
    { id: 'r3', name: 'Crystalline Iris', description: 'Forms perfect crystal petals that help organize chaotic thoughts into clarity.' },
    { id: 'r4', name: 'Thundervine Ivy', description: 'Crackles with electric energy, perfect for powering through challenging work.' },
    { id: 'r5', name: 'Mistwood Sage', description: 'Ancient wisdom flows through its leaves, guiding you toward wise decisions.' }
  ],
  epic: [
    { id: 'e1', name: 'Phoenix Blossom', description: 'Rises from ashes of abandoned tasks, granting renewed purpose and unstoppable momentum.' },
    { id: 'e2', name: 'Voidheart Lotus', description: 'Blooms in impossible spaces, making even the most daunting tasks feel achievable.' },
    { id: 'e3', name: 'Timebend Tulip', description: 'Warps perception of time, making hours feel like minutes during deep work.' },
    { id: 'e4', name: 'Dreamweaver Dahlia', description: 'Transforms visions into reality, perfect for creative and ambitious projects.' },
    { id: 'e5', name: 'Soulfire Sunflower', description: 'Burns with inner passion, aligning your work with your deepest purpose.' }
  ],
  extinct: [
    { id: 'x1', name: 'Eternity Bloom', description: 'Last of its kind, contains the concentrated will of ancient civilizations. Tasks completed under its watch become legendary.' },
    { id: 'x2', name: 'Cosmosis Carnation', description: 'Witnessed the birth of stars, grants cosmic perspective making any obstacle seem surmountable.' },
    { id: 'x3', name: 'Mythweave Magnolia', description: 'Extinct for millennia, its presence turns ordinary work into legendary achievements.' },
    { id: 'x4', name: 'Celestial Chrysanthemum', description: 'Bloomed in heavenly gardens, perfects everything it touches with divine grace.' },
    { id: 'x5', name: 'Primordial Peony', description: 'Oldest plant in existence, channels the raw creative force of the universe itself.' }
  ]
};

const REWARD_PROBABILITIES = {
  basic: { basic: 0.50, rare: 0.35, epic: 0.12, extinct: 0.03 },
  rare: { basic: 0.20, rare: 0.50, epic: 0.25, extinct: 0.05 },
  epic: { basic: 0.10, rare: 0.40, epic: 0.40, extinct: 0.10 },
  extinct: { rare: 0.20, epic: 0.50, extinct: 0.30 }
};

function ProductivityGarden() {
  const [seedBank, setSeedBank] = useState([]);
  const [gardenBeds, setGardenBeds] = useState([Array(10).fill(null), Array(10).fill(null)]);
  const [catalogue, setCatalogue] = useState({});
  const [showCatalogue, setShowCatalogue] = useState(false);
  const [selectedPlant, setSelectedPlant] = useState(null);
  const [hoveredPlant, setHoveredPlant] = useState(null);
  const [draggedSeed, setDraggedSeed] = useState(null);
  const [showTaskModal, setShowTaskModal] = useState(false);
  const [pendingPlant, setPendingPlant] = useState(null);
  const [taskTitle, setTaskTitle] = useState('');
  const [taskDuration, setTaskDuration] = useState('1');
  const [celebratingPlant, setCelebratingPlant] = useState(null);
  const [removingPlant, setRemovingPlant] = useState(null);

  useEffect(() => {
    const initialSeeds = [];
    for (let i = 0; i < 5; i++) {
      const randomPlant = PLANTS.basic[Math.floor(Math.random() * PLANTS.basic.length)];
      initialSeeds.push({ tier: 'basic', plantId: randomPlant.id });
    }
    setSeedBank(initialSeeds);
  }, []);

  useEffect(() => {
    const interval = setInterval(() => {
      setGardenBeds(prev => {
        const newBeds = prev.map(bed => 
          bed.map(plant => {
            if (!plant || plant.completed) return plant;
            
            const elapsed = Date.now() - plant.plantedAt;
            const phaseTime = (plant.duration * 60 * 1000) / 5;
            const newPhase = Math.min(4, Math.floor(elapsed / phaseTime));
            const isReady = elapsed >= plant.duration * 60 * 1000;
            
            return { ...plant, phase: newPhase, ready: isReady };
          })
        );
        return newBeds;
      });
    }, 1000);
    
    return () => clearInterval(interval);
  }, []);

  const plantSeed = (bedIndex, slotIndex, seedIndex = 0) => {
    if (seedBank.length === 0) {
      alert('No seeds available!');
      return;
    }

    // Open the task modal instead of using prompts
    setPendingPlant({ bedIndex, slotIndex, seedIndex });
    setShowTaskModal(true);
    setTaskTitle('');
    setTaskDuration('1');
  };

  const handleDragStart = (e, seedIndex) => {
    console.log('DRAG START:', seedIndex);
    setDraggedSeed(seedIndex);
    e.dataTransfer.effectAllowed = 'move';
    e.dataTransfer.setData('seedIndex', seedIndex.toString());
  };

  const handleDragOver = (e, bedIndex, slotIndex) => {
    e.preventDefault();
    e.stopPropagation();
    const plant = gardenBeds[bedIndex][slotIndex];
    if (!plant) {
      e.dataTransfer.dropEffect = 'move';
    } else {
      e.dataTransfer.dropEffect = 'none';
    }
  };

  const handleDrop = (e, bedIndex, slotIndex) => {
    console.log('=== DROP EVENT ===');
    e.preventDefault();
    e.stopPropagation();
    
    const seedIndexStr = e.dataTransfer.getData('seedIndex');
    const seedIndex = parseInt(seedIndexStr);
    
    console.log('seedIndex from dataTransfer:', seedIndex);
    
    const plant = gardenBeds[bedIndex][slotIndex];
    
    if (!isNaN(seedIndex) && seedIndex >= 0 && !plant && seedBank[seedIndex]) {
      console.log('✓ Opening task modal');
      
      // Store the pending plant info and show modal
      setPendingPlant({ bedIndex, slotIndex, seedIndex });
      setShowTaskModal(true);
      setTaskTitle('');
      setTaskDuration('1');
    }
    
    setDraggedSeed(null);
  };

  const confirmPlantTask = () => {
    if (!pendingPlant) return;
    
    const { bedIndex, slotIndex, seedIndex } = pendingPlant;
    
    if (!taskTitle.trim()) {
      alert('Please enter a task title');
      return;
    }
    
    const duration = parseInt(taskDuration);
    if (!duration || duration < 1 || duration > 60) {
      alert('Duration must be between 1 and 60 minutes');
      return;
    }
    
    const seed = seedBank[seedIndex];
    
    setSeedBank(prev => prev.filter((_, i) => i !== seedIndex));

    const newPlant = {
      taskTitle: taskTitle.trim(),
      duration,
      plantedAt: Date.now(),
      phase: 0,
      ready: false,
      completed: false,
      tier: seed.tier,
      plantId: seed.plantId
    };

    setGardenBeds(prev => {
      const newBeds = [...prev];
      newBeds[bedIndex] = [...newBeds[bedIndex]];
      newBeds[bedIndex][slotIndex] = newPlant;
      return newBeds;
    });
    
    // Close modal and reset
    setShowTaskModal(false);
    setPendingPlant(null);
    setTaskTitle('');
    setTaskDuration('1');
  };

  const completePlant = (bedIndex, slotIndex) => {
    const plant = gardenBeds[bedIndex][slotIndex];
    if (!plant || !plant.ready || plant.completed) return;

    // Show celebration animation
    setCelebratingPlant({ bedIndex, slotIndex, tier: plant.tier });
    
    setTimeout(() => {
      setCelebratingPlant(null);
    }, 2000);

    setGardenBeds(prev => {
      const newBeds = [...prev];
      newBeds[bedIndex] = [...newBeds[bedIndex]];
      newBeds[bedIndex][slotIndex] = { ...plant, completed: true, phase: 5 };
      return newBeds;
    });

    setCatalogue(prev => ({
      ...prev,
      [plant.plantId]: (prev[plant.plantId] || 0) + 1
    }));

    const probabilities = REWARD_PROBABILITIES[plant.tier];
    const rand = Math.random();
    let cumulative = 0;
    let rewardTier = 'basic';

    for (const [tier, prob] of Object.entries(probabilities)) {
      cumulative += prob;
      if (rand <= cumulative) {
        rewardTier = tier;
        break;
      }
    }

    if (seedBank.length < 10) {
      const tierPlants = PLANTS[rewardTier];
      const randomPlant = tierPlants[Math.floor(Math.random() * tierPlants.length)];
      setSeedBank(prev => [...prev, { tier: rewardTier, plantId: randomPlant.id }]);
    }
  };

  const removePlant = (bedIndex, slotIndex) => {
    const plant = gardenBeds[bedIndex][slotIndex];
    if (!plant) return;

    // Show withering animation
    setRemovingPlant({ bedIndex, slotIndex });
    
    // Instantly remove the plant
    if (plant.completed) {
      setCatalogue(prev => ({
        ...prev,
        [plant.plantId]: Math.max(0, (prev[plant.plantId] || 0) - 1)
      }));
    }

    setGardenBeds(prev => {
      const newBeds = [...prev];
      newBeds[bedIndex] = [...newBeds[bedIndex]];
      newBeds[bedIndex][slotIndex] = null;
      return newBeds;
    });
    
    // Clear animation state after animation completes
    setTimeout(() => {
      setRemovingPlant(null);
    }, 1000);
  };

  const getPlantDisplay = (plant) => {
    if (!plant) return null;
    
    const elapsed = Date.now() - plant.plantedAt;
    const totalTime = plant.duration * 60 * 1000;
    const progress = Math.min(100, (elapsed / totalTime) * 100);
    
    // Get the icon progression for this specific plant
    const icons = PLANT_ICONS[plant.plantId] || ['🌱', '🌿', '🪴', '🌸', '🌷'];
    const displayIcon = plant.completed ? icons[4] : icons[plant.phase];
    
    return (
      <div className="relative w-full h-full flex flex-col items-center justify-end pb-4">
        <div 
          className="text-7xl mb-2"
          style={{ 
            filter: plant.completed ? `drop-shadow(0 0 12px ${TIER_COLORS[plant.tier]}) drop-shadow(0 0 20px ${TIER_COLORS[plant.tier]})` : 'drop-shadow(2px 2px 3px rgba(0,0,0,0.3))'
          }}
        >
          {displayIcon}
        </div>
        {!plant.completed && (
          <div className="w-24 h-2.5 bg-stone-800 rounded-full overflow-hidden shadow-inner border border-stone-900">
            <div 
              className="h-full bg-gradient-to-r from-green-400 to-green-500 transition-all duration-1000 shadow-sm"
              style={{ width: `${progress}%` }}
            />
          </div>
        )}
        {plant.ready && !plant.completed && (
          <div className="absolute top-3 left-3 w-5 h-5 bg-green-400 rounded-full animate-pulse shadow-lg border-2 border-green-300" />
        )}
      </div>
    );
  };

  return (
    <div className="min-h-screen p-8 relative overflow-hidden" style={{
      background: 'linear-gradient(180deg, #87ceeb 0%, #98d8e8 20%, #a8e6cf 40%, #7fb069 60%, #6b9e5a 80%, #5a8c4a 100%)',
    }}>
      {/* Animated clouds */}
      <div className="absolute inset-0 opacity-30 pointer-events-none">
        <div className="absolute top-10 left-20 text-6xl animate-float" style={{animationDelay: '0s'}}>☁️</div>
        <div className="absolute top-32 right-40 text-5xl animate-float" style={{animationDelay: '2s'}}>☁️</div>
        <div className="absolute top-20 left-1/2 text-7xl animate-float" style={{animationDelay: '4s'}}>☁️</div>
        <div className="absolute top-40 left-1/3 text-6xl animate-float" style={{animationDelay: '1s'}}>☁️</div>
        <div className="absolute top-60 right-20 text-5xl animate-float" style={{animationDelay: '3s'}}>☁️</div>
      </div>

      {/* Sun */}
      <div className="absolute top-10 right-10 text-8xl animate-pulse opacity-90" style={{animationDuration: '3s'}}>
        ☀️
      </div>

      {/* Birds */}
      <div className="absolute inset-0 pointer-events-none">
        <div className="absolute top-24 left-1/4 text-3xl animate-float" style={{animationDelay: '0s', animationDuration: '8s'}}>🦅</div>
        <div className="absolute top-40 right-1/3 text-2xl animate-float" style={{animationDelay: '2s', animationDuration: '10s'}}>🕊️</div>
        <div className="absolute top-32 left-2/3 text-2xl animate-float" style={{animationDelay: '4s', animationDuration: '9s'}}>🦜</div>
      </div>

      {/* Butterflies */}
      <div className="absolute inset-0 pointer-events-none">
        <div className="absolute top-1/3 left-10 text-4xl" style={{animation: 'butterfly 6s ease-in-out infinite', animationDelay: '0s'}}>🦋</div>
        <div className="absolute top-1/2 right-16 text-3xl" style={{animation: 'butterfly 7s ease-in-out infinite', animationDelay: '2s'}}>🦋</div>
        <div className="absolute top-2/3 left-1/4 text-3xl" style={{animation: 'butterfly 8s ease-in-out infinite', animationDelay: '4s'}}>🦋</div>
      </div>

      {/* Grass texture overlay */}
      <div className="absolute inset-0 opacity-20" style={{
        backgroundImage: `
          repeating-linear-gradient(90deg, transparent, transparent 2px, rgba(80, 120, 50, 0.3) 2px, rgba(80, 120, 50, 0.3) 4px),
          repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(60, 100, 40, 0.2) 2px, rgba(60, 100, 40, 0.2) 4px)
        `
      }}></div>

      {/* Small grass details */}
      <div className="absolute inset-0 opacity-15" style={{
        backgroundImage: 'radial-gradient(ellipse 1px 3px at 20% 30%, #4a7c3f 50%, transparent 50%), radial-gradient(ellipse 1px 3px at 60% 70%, #5a8c4f 50%, transparent 50%), radial-gradient(ellipse 1px 3px at 45% 15%, #4a7c3f 50%, transparent 50%)',
        backgroundSize: '50px 50px, 70px 70px, 90px 90px'
      }}></div>

      <div className="max-w-7xl mx-auto relative z-10">
        <h1 className="text-6xl font-bold text-amber-100 mb-3 text-center" style={{
          textShadow: '3px 3px 6px rgba(0,0,0,0.5), -1px -1px 0 rgba(139, 69, 19, 0.8)'
        }}>
          🌿 Productive Grow a Garden 🌿
        </h1>
        <p className="text-center text-amber-50 mb-2 text-xl" style={{textShadow: '2px 2px 4px rgba(0,0,0,0.5)'}}>
          Grow your tasks into magical plants
        </p>
        <p className="text-center text-amber-200 mb-10 text-lg italic" style={{textShadow: '2px 2px 4px rgba(0,0,0,0.5)'}}>
          by: CD
        </p>

        {/* Seed Bank */}
        <div className="rounded-2xl p-6 mb-12 relative" style={{
          background: 'linear-gradient(135deg, #d4a574 0%, #b8935f 50%, #a67c52 100%)',
          boxShadow: 'inset 0 2px 4px rgba(255,255,255,0.3), 0 8px 20px rgba(0,0,0,0.4)',
          border: '4px solid #6d4c2e'
        }}>
          <h2 className="text-2xl font-bold mb-4 flex items-center gap-3" style={{color: '#4a2c1a', textShadow: '1px 1px 2px rgba(255,255,255,0.3)'}}>
            <Sprout size={28} />
            Seed Bank ({seedBank.length}/10)
          </h2>
                        <div className="flex gap-3 flex-wrap">
            {seedBank.map((seed, i) => (
              <div 
                key={i} 
                className="w-16 h-16 rounded-xl flex items-center justify-center relative transition-all" 
                style={{
                  background: 'linear-gradient(135deg, #e8c392 0%, #d4a574 100%)',
                  boxShadow: 'inset 0 2px 4px rgba(0,0,0,0.2), 0 4px 8px rgba(0,0,0,0.3)',
                  border: '3px solid #8b6f47',
                  opacity: draggedSeed === i ? 0.5 : 1,
                  cursor: 'grab',
                  transform: draggedSeed === i ? 'scale(0.9)' : 'scale(1)'
                }}
                draggable
                onDragStart={(e) => handleDragStart(e, i)}
                onDragEnd={() => setDraggedSeed(null)}
              >
                <span className="text-3xl" style={{filter: 'drop-shadow(1px 1px 2px rgba(0,0,0,0.3)', pointerEvents: 'none'}}>🌰</span>
              </div>
            ))}
            {Array(10 - seedBank.length).fill(0).map((_, i) => (
              <div key={`empty-${i}`} className="w-16 h-16 rounded-xl border-3 border-dashed" style={{
                background: 'rgba(212, 165, 116, 0.3)',
                borderColor: '#8b6f47'
              }} />
            ))}
          </div>
        </div>

        {/* Garden Beds */}
        {gardenBeds.map((bed, bedIndex) => (
          <div key={bedIndex} className="mb-12 relative">
            {/* Decorative elements around garden bed */}
            <div className="absolute -left-16 top-1/4 text-6xl opacity-80" style={{transform: 'rotate(-15deg)'}}>🌻</div>
            <div className="absolute -right-16 top-1/3 text-5xl opacity-80" style={{transform: 'rotate(15deg)'}}>🌺</div>
            <div className="absolute -left-12 top-2/3 text-4xl opacity-70">🍄</div>
            <div className="absolute -right-12 top-1/2 text-5xl opacity-75">🌼</div>
            <div className="absolute left-8 -top-8 text-3xl opacity-60">🐛</div>
            <div className="absolute right-16 -top-6 text-3xl opacity-60">🐝</div>
            <div className="absolute left-1/3 -bottom-8 text-4xl opacity-70">🪨</div>
            <div className="absolute right-1/4 -bottom-6 text-3xl opacity-70">🪴</div>
            
            {/* Garden bed area with integrated fence */}
            <div className="rounded-3xl p-10 relative" style={{
              background: 'linear-gradient(135deg, #8b4513 0%, #a0522d 20%, #cd853f 40%, #daa520 60%, #f4a460 80%, #d2691e 100%)',
              boxShadow: 'inset 0 6px 12px rgba(0,0,0,0.3), 0 12px 35px rgba(0,0,0,0.5)',
              border: '8px solid #3d2415',
              borderRadius: '30px'
            }}>
              {/* Decorative wood grain texture */}
              <div className="absolute inset-0 rounded-3xl opacity-20 pointer-events-none" style={{
                backgroundImage: `repeating-linear-gradient(90deg, transparent, transparent 3px, rgba(101, 67, 33, 0.5) 3px, rgba(101, 67, 33, 0.5) 6px)`
              }}></div>
              
              {/* Corner decorations */}
              <div className="absolute top-4 left-4 text-3xl opacity-70">🌿</div>
              <div className="absolute top-4 right-4 text-3xl opacity-70">🌿</div>
              <div className="absolute bottom-4 left-4 text-3xl opacity-70">🍂</div>
              <div className="absolute bottom-4 right-4 text-3xl opacity-70">🍂</div>
              <h3 className="text-2xl font-bold mb-6 text-center" style={{
                color: '#3d2817',
                textShadow: '1px 1px 2px rgba(255,255,255,0.4)'
              }}>
                Garden Bed {bedIndex + 1}
              </h3>

              {/* Soil plots */}
              <div className="grid grid-cols-5 gap-5 relative z-10">
                {bed.map((plant, slotIndex) => (
                  <div
                    key={slotIndex}
                    className="relative rounded-3xl transition-all"
                    style={{
                      height: '180px',
                      background: 'radial-gradient(circle at 30% 30%, #5a3a1f 0%, #4a2f19 40%, #3a2514 100%)',
                      boxShadow: draggedSeed !== null && !plant 
                        ? 'inset 0 8px 16px rgba(0,0,0,0.6), 0 6px 12px rgba(0,0,0,0.4), 0 0 25px rgba(251, 191, 36, 0.7), inset 0 0 20px rgba(251, 191, 36, 0.3)'
                        : 'inset 0 8px 16px rgba(0,0,0,0.6), 0 6px 12px rgba(0,0,0,0.4)',
                      border: draggedSeed !== null && !plant ? '4px solid #fbbf24' : '4px solid #2d1810',
                      backgroundImage: `
                        radial-gradient(circle at 25% 30%, rgba(90, 58, 31, 0.7) 3px, transparent 3px),
                        radial-gradient(circle at 75% 70%, rgba(74, 47, 25, 0.6) 2px, transparent 2px),
                        radial-gradient(circle at 50% 50%, rgba(109, 76, 46, 0.5) 2px, transparent 2px),
                        radial-gradient(ellipse at 60% 40%, rgba(139, 90, 43, 0.3) 4px, transparent 4px)
                      `,
                      backgroundSize: '25px 25px, 18px 18px, 30px 30px, 35px 35px',
                      cursor: plant ? 'default' : 'pointer',
                      position: 'relative',
                      overflow: 'hidden'
                    }}
                    onClick={() => !plant && draggedSeed === null && plantSeed(bedIndex, slotIndex)}
                    onDragOver={(e) => handleDragOver(e, bedIndex, slotIndex)}
                    onDrop={(e) => handleDrop(e, bedIndex, slotIndex)}
                    onMouseEnter={() => plant && setHoveredPlant({ bedIndex, slotIndex })}
                    onMouseLeave={() => setHoveredPlant(null)}
                  >
                    {/* Subtle glow effect for empty slots */}
                    {!plant && (
                      <div className="absolute inset-0 rounded-3xl" style={{
                        background: 'radial-gradient(circle at 50% 50%, rgba(139, 90, 43, 0.2) 0%, transparent 70%)',
                        animation: 'pulse 3s ease-in-out infinite'
                      }}></div>
                    )}
                    {/* Removal animation overlay - positioned within the slot */}
                    {removingPlant?.bedIndex === bedIndex && removingPlant?.slotIndex === slotIndex && (
                      <div className="absolute inset-0 pointer-events-none z-50 flex items-center justify-center">
                        <div 
                          className="absolute text-9xl"
                          style={{
                            animation: 'explodeOut 1s ease-out',
                            opacity: 0
                          }}
                        >
                          💥
                        </div>
                      </div>
                    )}

                    {plant ? (
                      <>
                        
                        {/* Celebration animation overlay */}
                        {celebratingPlant?.bedIndex === bedIndex && celebratingPlant?.slotIndex === slotIndex && (
                          <div className="absolute inset-0 pointer-events-none z-50 flex items-center justify-center">
                            {celebratingPlant.tier === 'basic' && (
                              <>
                                <div className="absolute text-6xl animate-ping" style={{animationDuration: '1s'}}>✨</div>
                                <div className="absolute text-4xl" style={{
                                  animation: 'bounce 0.5s ease infinite',
                                  color: TIER_COLORS.basic,
                                  textShadow: `0 0 20px ${TIER_COLORS.basic}`
                                }}>🌿</div>
                              </>
                            )}
                            {celebratingPlant.tier === 'rare' && (
                              <>
                                <div className="absolute text-7xl animate-spin" style={{animationDuration: '1s'}}>💫</div>
                                <div className="absolute text-5xl animate-pulse" style={{
                                  color: TIER_COLORS.rare,
                                  textShadow: `0 0 30px ${TIER_COLORS.rare}`
                                }}>⭐</div>
                              </>
                            )}
                            {celebratingPlant.tier === 'epic' && (
                              <>
                                <div className="absolute text-8xl animate-ping" style={{animationDuration: '0.5s'}}>💥</div>
                                <div className="absolute text-6xl" style={{
                                  animation: 'spin 0.8s linear infinite',
                                  color: TIER_COLORS.epic,
                                  textShadow: `0 0 40px ${TIER_COLORS.epic}, 0 0 60px ${TIER_COLORS.epic}`
                                }}>🌟</div>
                              </>
                            )}
                            {celebratingPlant.tier === 'extinct' && (
                              <>
                                <div className="absolute text-9xl animate-ping" style={{animationDuration: '0.3s'}}>💥</div>
                                <div className="absolute text-7xl animate-bounce" style={{
                                  color: TIER_COLORS.extinct,
                                  textShadow: `0 0 50px ${TIER_COLORS.extinct}, 0 0 80px ${TIER_COLORS.extinct}, 0 0 100px ${TIER_COLORS.extinct}`
                                }}>🔥</div>
                              </>
                            )}
                          </div>
                        )}
                        
                        <div 
                          className="w-full h-full"
                          onClick={(e) => {
                            if (plant.ready && !plant.completed) {
                              e.stopPropagation();
                              completePlant(bedIndex, slotIndex);
                            }
                          }}
                        >
                          {getPlantDisplay(plant)}
                        </div>
                        <button
                          onClick={(e) => {
                            e.stopPropagation();
                            removePlant(bedIndex, slotIndex);
                          }}
                          className="absolute top-3 right-3 bg-red-600 text-white rounded-full p-2 hover:bg-red-700 transition-all hover:scale-110 z-30"
                          style={{boxShadow: '0 4px 8px rgba(0,0,0,0.4)'}}
                        >
                          <Trash2 size={16} />
                        </button>
                        {hoveredPlant?.bedIndex === bedIndex && hoveredPlant?.slotIndex === slotIndex && (
                          <div 
                            className="absolute -top-16 left-1/2 transform -translate-x-1/2 px-4 py-2 rounded-xl text-sm whitespace-nowrap z-40 font-bold border-2"
                            style={{ 
                              backgroundColor: 'rgba(0,0,0,0.95)',
                              color: TIER_COLORS[plant.tier],
                              borderColor: TIER_COLORS[plant.tier],
                              boxShadow: '0 4px 12px rgba(0,0,0,0.5)'
                            }}
                          >
                            {plant.taskTitle}
                          </div>
                        )}
                      </>
                    ) : (
                      <div className="w-full h-full flex items-center justify-center">
                        <div className="text-7xl font-bold transition-all hover:scale-110" style={{
                          color: '#8b6f47',
                          textShadow: '2px 2px 4px rgba(0,0,0,0.5)',
                          opacity: 0.7
                        }}>
                          +
                        </div>
                      </div>
                    )}
                    
                    {/* Small sparkles in empty soil */}
                    {!plant && (
                      <>
                        <div className="absolute top-4 left-4 w-1.5 h-1.5 bg-amber-400 rounded-full opacity-60" style={{boxShadow: '0 0 4px #fbbf24'}}></div>
                        <div className="absolute top-8 right-6 w-1 h-1 bg-amber-300 rounded-full opacity-50" style={{boxShadow: '0 0 3px #fcd34d'}}></div>
                      </>
                    )}
                  </div>
                ))}
              </div>
            </div>
          </div>
        ))}

        {/* Catalogue Button */}
        <button
          onClick={() => setShowCatalogue(true)}
          className="fixed bottom-10 right-10 text-white rounded-full p-5 transition-all z-40 hover:scale-110 transform"
          style={{
            background: 'linear-gradient(135deg, #9333ea 0%, #7e22ce 100%)',
            boxShadow: '0 8px 20px rgba(147, 51, 234, 0.4), inset 0 1px 2px rgba(255,255,255,0.3)'
          }}
        >
          <Book size={36} />
        </button>

        {/* Catalogue Modal */}
        {showCatalogue && (
          <div className="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50 p-4">
            <div className="rounded-3xl p-8 max-w-6xl w-full h-[85vh] flex flex-col relative" style={{
              background: 'linear-gradient(135deg, #fef3c7 0%, #fde68a 100%)',
              boxShadow: '0 20px 60px rgba(0,0,0,0.5), inset 0 2px 4px rgba(255,255,255,0.5)',
              border: '5px solid #78350f'
            }}>
              
              <h2 className="text-4xl font-bold mb-6" style={{color: '#78350f', textShadow: '2px 2px 4px rgba(255,255,255,0.5)'}}>
                🌺 Plant Catalogue 🌺
              </h2>
              
              <div className="grid grid-cols-2 gap-6 flex-1 overflow-hidden">
                {Object.entries(PLANTS).map(([tier, plants]) => (
                  <div key={tier} className="flex flex-col">
                    <h3 
                      className="text-2xl font-bold mb-4 capitalize"
                      style={{ 
                        color: TIER_COLORS[tier],
                        textShadow: '1px 1px 3px rgba(0,0,0,0.3)'
                      }}
                    >
                      {tier} Tier
                    </h3>
                    <div className="grid grid-cols-5 gap-3">
                      {plants.map(plant => {
                        const count = catalogue[plant.id] || 0;
                        const discovered = count > 0;
                        const icons = PLANT_ICONS[plant.id] || ['🌱', '🌿', '🪴', '🌸', '🌷'];
                        const finalIcon = icons[4]; // Show the final form in catalogue
                        
                        return (
                          <div
                            key={plant.id}
                            onClick={() => discovered && setSelectedPlant(plant)}
                            className="relative rounded-xl p-3 transition-all"
                            style={{
                              background: discovered 
                                ? 'linear-gradient(135deg, #ffffff 0%, #fef3c7 100%)'
                                : 'linear-gradient(135deg, #d1d5db 0%, #9ca3af 100%)',
                              border: discovered ? '3px solid #d97706' : '3px solid #6b7280',
                              boxShadow: discovered 
                                ? '0 4px 12px rgba(217, 119, 6, 0.3), inset 0 1px 2px rgba(255,255,255,0.5)'
                                : '0 2px 6px rgba(0,0,0,0.2)',
                              cursor: discovered ? 'pointer' : 'default'
                            }}
                            onMouseEnter={(e) => {
                              if (discovered) e.currentTarget.style.transform = 'scale(1.05)';
                            }}
                            onMouseLeave={(e) => {
                              e.currentTarget.style.transform = 'scale(1)';
                            }}
                          >
                            <div className="text-3xl mb-2 text-center" style={{
                              filter: 'drop-shadow(2px 2px 4px rgba(0,0,0,0.2))'
                            }}>
                              {discovered ? finalIcon : '❓'}
                            </div>
                            {discovered && (
                              <div 
                                className="absolute top-2 right-2 rounded-full px-2 py-0.5 text-xs font-bold text-white"
                                style={{ 
                                  backgroundColor: TIER_COLORS[tier],
                                  boxShadow: '0 2px 6px rgba(0,0,0,0.3)'
                                }}
                              >
                                {count}
                              </div>
                            )}
                          </div>
                        );
                      })}
                    </div>
                  </div>
                ))}
              </div>
              
              <div className="mt-6 text-center">
                <button
                  onClick={() => setShowCatalogue(false)}
                  className="px-8 py-3 rounded-xl font-bold text-white text-lg transition-all hover:scale-105"
                  style={{
                    background: 'linear-gradient(135deg, #dc2626 0%, #b91c1c 100%)',
                    boxShadow: '0 4px 12px rgba(220, 38, 38, 0.4)'
                  }}
                >
                  Close Catalogue
                </button>
              </div>
            </div>
          </div>
        )}

        {/* Plant Detail Modal */}
        {selectedPlant && (
          <div 
            className="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50 p-4"
            onClick={() => setSelectedPlant(null)}
          >
            <div 
              className="rounded-3xl p-10 max-w-lg w-full relative" 
              style={{
                background: 'linear-gradient(135deg, #ffffff 0%, #fef3c7 100%)',
                boxShadow: '0 20px 60px rgba(0,0,0,0.5), inset 0 2px 4px rgba(255,255,255,0.5)',
                border: '5px solid #78350f'
              }}
              onClick={(e) => e.stopPropagation()}
            >
              <button
                onClick={(e) => {
                  e.stopPropagation();
                  setSelectedPlant(null);
                }}
                className="absolute bg-red-500 text-white rounded-full transition-all hover:scale-110 hover:bg-red-600 flex items-center justify-center cursor-pointer z-50"
                style={{
                  boxShadow: '0 4px 12px rgba(0,0,0,0.4)',
                  width: '56px',
                  height: '56px',
                  top: '20px',
                  right: '20px',
                  padding: 0
                }}
              >
                <X size={28} strokeWidth={3} />
              </button>
              
              <div className="text-7xl mb-6 text-center" style={{filter: 'drop-shadow(3px 3px 6px rgba(0,0,0,0.2))'}}>
                {PLANT_ICONS[selectedPlant.id] ? PLANT_ICONS[selectedPlant.id][4] : '🌺'}
              </div>
              <h3 className="text-3xl font-bold mb-4 text-center" style={{
                color: '#78350f',
                textShadow: '1px 1px 2px rgba(255,255,255,0.5)'
              }}>
                {selectedPlant.name}
              </h3>
              <p className="text-center italic text-lg leading-relaxed" style={{color: '#44403c'}}>
                {selectedPlant.description}
              </p>
            </div>
          </div>
        )}

        {/* Task Input Modal */}
        {showTaskModal && (
          <div className="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50 p-4">
            <div className="rounded-3xl p-10 max-w-lg w-full relative" style={{
              background: 'linear-gradient(135deg, #ffffff 0%, #fef3c7 100%)',
              boxShadow: '0 20px 60px rgba(0,0,0,0.5), inset 0 2px 4px rgba(255,255,255,0.5)',
              border: '5px solid #78350f'
            }}>
              <h3 className="text-3xl font-bold mb-6 text-center" style={{
                color: '#78350f',
                textShadow: '1px 1px 2px rgba(255,255,255,0.5)'
              }}>
                🌱 Plant Your Seed
              </h3>
              
              <div className="mb-6">
                <label className="block text-lg font-bold mb-2" style={{color: '#78350f'}}>
                  Task Title:
                </label>
                <input
                  type="text"
                  value={taskTitle}
                  onChange={(e) => setTaskTitle(e.target.value)}
                  placeholder="e.g., Write report, Study math..."
                  className="w-full px-4 py-3 rounded-xl text-lg border-3"
                  style={{
                    border: '3px solid #d97706',
                    background: '#fef3c7'
                  }}
                  autoFocus
                />
              </div>
              
              <div className="mb-8">
                <label className="block text-lg font-bold mb-2" style={{color: '#78350f'}}>
                  Duration (1-60 minutes):
                </label>
                <input
                  type="number"
                  value={taskDuration}
                  onChange={(e) => setTaskDuration(e.target.value)}
                  min="1"
                  max="60"
                  className="w-full px-4 py-3 rounded-xl text-lg border-3"
                  style={{
                    border: '3px solid #d97706',
                    background: '#fef3c7'
                  }}
                />
              </div>
              
              <div className="flex gap-4">
                <button
                  onClick={() => {
                    setShowTaskModal(false);
                    setPendingPlant(null);
                    setTaskTitle('');
                    setTaskDuration('1');
                  }}
                  className="flex-1 px-6 py-3 rounded-xl font-bold text-white text-lg transition-all hover:scale-105"
                  style={{
                    background: 'linear-gradient(135deg, #6b7280 0%, #4b5563 100%)',
                    boxShadow: '0 4px 12px rgba(107, 114, 128, 0.4)'
                  }}
                >
                  Cancel
                </button>
                <button
                  onClick={confirmPlantTask}
                  className="flex-1 px-6 py-3 rounded-xl font-bold text-white text-lg transition-all hover:scale-105"
                  style={{
                    background: 'linear-gradient(135deg, #10b981 0%, #059669 100%)',
                    boxShadow: '0 4px 12px rgba(16, 185, 129, 0.4)'
                  }}
                >
                  Plant It! 🌱
                </button>
              </div>
            </div>
          </div>
        )}
      </div>

      {/* Add CSS animations */}
      <style>{`
        @keyframes fadeOut {
          0% { opacity: 1; transform: scale(1); }
          100% { opacity: 0; transform: scale(0.3); }
        }
        @keyframes explodeOut {
          0% { opacity: 1; transform: scale(0.3); }
          50% { opacity: 0.9; transform: scale(2.2); }
          100% { opacity: 0; transform: scale(0.8); }
        }
        @keyframes float {
          0%, 100% { transform: translateY(0px) translateX(0px); }
          50% { transform: translateY(-20px) translateX(30px); }
        }
        @keyframes butterfly {
          0%, 100% { transform: translate(0, 0) rotate(0deg); }
          25% { transform: translate(30px, -40px) rotate(10deg); }
          50% { transform: translate(60px, -20px) rotate(-10deg); }
          75% { transform: translate(30px, 20px) rotate(5deg); }
        }
      `}</style>
    </div>
  );
}

export default ProductivityGarden;