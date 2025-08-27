<script>
  import { onMount, onDestroy } from 'svelte';
  
  // Целевая дата: 30.08.2025
  const targetDate = new Date('2025-08-30T00:00:00');
  
  let timeLeft = $state({
    days: 0,
    hours: 0,
    minutes: 0,
    seconds: 0,
    isExpired: false
  });
  
  let interval;
  
  const updateTimer = () => {
    const now = new Date();
    const difference = targetDate.getTime() - now.getTime();
    
    if (difference <= 0) {
      timeLeft = {
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0,
        isExpired: true
      };
      return;
    }
    
    const days = Math.floor(difference / (1000 * 60 * 60 * 24));
    const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((difference % (1000 * 60)) / 1000);
    
    timeLeft = {
      days,
      hours,
      minutes,
      seconds,
      isExpired: false
    };
  };
  
  onMount(() => {
    updateTimer(); // Обновить сразу
    interval = setInterval(updateTimer, 1000); // Обновлять каждую секунду
  });
  
  onDestroy(() => {
    if (interval) {
      clearInterval(interval);
    }
  });
</script>

<div class="countdown-app">
  <div class="background-pattern"></div>
  <div class="main-content">
    <div class="header-section">
      <div class="title-wrapper">
        <h1 class="main-title">🍾 EVENT 🍾</h1>
        <div class="target-date">30 August 2025</div>
      </div>
    </div>
    
    {#if timeLeft.isExpired}
             <div class="expired-state">
         <div class="expired-icon">⏰</div>
         <h2>Event Completed</h2>
         <p>Time expired on August 30, 2025</p>
       </div>
    {:else}
      <div class="timer-grid">
                 <div class="time-card days">
           <div class="time-value">{timeLeft.days}</div>
           <div class="time-label">DAYS</div>
           <div class="progress-bar">
             <div class="progress-fill" style="width: {Math.min(timeLeft.days / 365 * 100, 100)}%"></div>
           </div>
         </div>
         
         <div class="time-card hours">
           <div class="time-value">{timeLeft.hours.toString().padStart(2, '0')}</div>
           <div class="time-label">HOURS</div>
           <div class="progress-bar">
             <div class="progress-fill" style="width: {timeLeft.hours / 24 * 100}%"></div>
           </div>
         </div>
         
         <div class="time-card minutes">
           <div class="time-value">{timeLeft.minutes.toString().padStart(2, '0')}</div>
           <div class="time-label">MINUTES</div>
           <div class="progress-bar">
             <div class="progress-fill" style="width: {timeLeft.minutes / 60 * 100}%"></div>
           </div>
         </div>
         
         <div class="time-card seconds">
           <div class="time-value">{timeLeft.seconds.toString().padStart(2, '0')}</div>
           <div class="time-label">SECONDS</div>
           <div class="progress-bar">
             <div class="progress-fill" style="width: {timeLeft.seconds / 60 * 100}%"></div>
           </div>
         </div>
      </div>
      
             <div class="stats-section">
         <div class="stat-item">
           <div class="stat-number">{Math.floor((targetDate.getTime() - new Date().getTime()) / (1000 * 60 * 60))}</div>
           <div class="stat-text">Hours remaining</div>
         </div>
         <div class="stat-item">
           <div class="stat-number">{Math.floor((targetDate.getTime() - new Date().getTime()) / (1000 * 60))}</div>
           <div class="stat-text">Minutes remaining</div>
         </div>
       </div>
     {/if}
     
     <div class="event-info">
       <div class="info-card">
         <div class="info-item">
           <div class="info-icon">🕐</div>
                       <div class="info-content">
              <div class="info-title">Corporate event starts: 1:00 PM</div>
              <div class="info-subtitle">(please arrive on time so we can prepare everything)</div>
            </div>
          </div>
          <div class="info-item">
            <div class="info-icon">🌍</div>
            <div class="info-content">
              <div class="info-title">Astoria Riverside</div>
              <div class="info-subtitle">(116 Pobediteley Avenue)</div>
            </div>
         </div>
       </div>
     </div>
   </div>
 </div>

<style>
  .countdown-app {
    min-height: 100vh;
    font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: linear-gradient(135deg, #1a1a1a 0%, #2d1b1b 50%, #ff6b35 100%);
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .background-pattern {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: 
      radial-gradient(circle at 25% 25%, rgba(255, 255, 255, 0.1) 0%, transparent 50%),
      radial-gradient(circle at 75% 75%, rgba(255, 255, 255, 0.05) 0%, transparent 50%);
    animation: patternMove 20s ease-in-out infinite alternate;
  }
  
  .main-content {
    position: relative;
    z-index: 1;
    text-align: center;
    padding: 2rem;
    max-width: 1200px;
    width: 100%;
  }
  
  .header-section {
    margin-bottom: 3rem;
  }
  
  .title-wrapper {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(20px);
    border-radius: 24px;
    padding: 2rem 3rem;
    border: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  }
  
  .main-title {
    font-size: 3.5rem;
    font-weight: 800;
    margin: 0 0 1rem 0;
    color: #ffffff;
    letter-spacing: -2px;
    text-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .target-date {
    font-size: 1.2rem;
    color: rgba(255, 255, 255, 0.8);
    font-weight: 500;
    letter-spacing: 2px;
  }
  
  .timer-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 2rem;
    margin-bottom: 3rem;
  }
  
  .time-card {
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(20px);
    border-radius: 20px;
    padding: 2.5rem 1.5rem;
    border: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    overflow: hidden;
  }
  
  
  .time-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
  }
  
  
  .time-value {
    font-size: 4rem;
    font-weight: 900;
    color: #ffffff;
    margin-bottom: 0.5rem;
    line-height: 1;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .time-label {
    font-size: 0.9rem;
    color: rgba(255, 255, 255, 0.7);
    font-weight: 600;
    letter-spacing: 2px;
    margin-bottom: 1.5rem;
  }
  
  .progress-bar {
    width: 100%;
    height: 6px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 3px;
    overflow: hidden;
    position: relative;
  }
  
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #ffffff, rgba(255, 255, 255, 0.8));
    border-radius: 3px;
    transition: width 0.3s ease;
    position: relative;
  }
  
  .progress-fill::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
    animation: progressShine 2s infinite;
  }
  
  .stats-section {
    display: flex;
    justify-content: center;
    gap: 4rem;
    margin-top: 2rem;
  }
  
  .stat-item {
    text-align: center;
  }
  
  .stat-number {
    font-size: 2rem;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 0.5rem;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .stat-text {
    font-size: 0.9rem;
    color: rgba(255, 255, 255, 0.7);
    font-weight: 500;
  }
  
  .expired-state {
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(20px);
    border-radius: 24px;
    padding: 4rem 3rem;
    border: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  .expired-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
    opacity: 0.8;
  }
  
  .expired-state h2 {
    font-size: 2.5rem;
    font-weight: 700;
    color: #ffffff;
    margin: 0 0 1rem 0;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .expired-state p {
    font-size: 1.2rem;
    color: rgba(255, 255, 255, 0.8);
    margin: 0;
    font-weight: 500;
  }
  
  @keyframes patternMove {
    0% { transform: translateX(-10px) translateY(-10px); }
    100% { transform: translateX(10px) translateY(10px); }
  }
  
     @keyframes progressShine {
     0% { transform: translateX(-100%); }
     100% { transform: translateX(100%); }
   }
   
   .event-info {
     margin-top: 3rem;
   }
   
   .info-card {
     background: rgba(255, 255, 255, 0.15);
     backdrop-filter: blur(20px);
     border-radius: 20px;
     padding: 2rem;
     border: 1px solid rgba(255, 255, 255, 0.2);
     box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
     max-width: 600px;
     margin: 0 auto;
   }
   
   .info-item {
     display: flex;
     align-items: flex-start;
     gap: 1rem;
     margin-bottom: 1.5rem;
   }
   
   .info-item:last-child {
     margin-bottom: 0;
   }
   
   .info-icon {
     font-size: 1.5rem;
     margin-top: 0.2rem;
     flex-shrink: 0;
   }
   
   .info-content {
     flex: 1;
   }
   
   .info-title {
     font-size: 1.1rem;
     font-weight: 600;
     color: #ffffff;
     margin-bottom: 0.3rem;
     text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
   }
   
   .info-subtitle {
     font-size: 0.9rem;
     color: rgba(255, 255, 255, 0.7);
     line-height: 1.4;
   }
   
   @media (max-width: 768px) {
    .main-title {
      font-size: 2.5rem;
      letter-spacing: -1px;
    }
    
    .title-wrapper {
      padding: 1.5rem 2rem;
    }
    
    .timer-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 1.5rem;
    }
    
    .time-card {
      padding: 2rem 1rem;
    }
    
    .time-value {
      font-size: 3rem;
    }
    
    .stats-section {
      gap: 2rem;
      flex-wrap: wrap;
    }
    
         .stat-number {
       font-size: 1.5rem;
     }
     
     .info-card {
       padding: 1.5rem;
     }
     
     .info-item {
       gap: 0.8rem;
     }
     
     .info-icon {
       font-size: 1.3rem;
     }
     
     .info-title {
       font-size: 1rem;
     }
     
     .info-subtitle {
       font-size: 0.85rem;
     }
   }
   
   @media (max-width: 480px) {
    .timer-grid {
      grid-template-columns: 1fr;
    }
    
    .main-title {
      font-size: 2rem;
    }
    
    .time-value {
      font-size: 2.5rem;
    }
  }
</style>
