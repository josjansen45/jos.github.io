import React, { useState, useEffect, useRef } from 'react';
import { 
  TrendingUp, 
  Shield, 
  Zap, 
  Globe, 
  ChevronRight, 
  Play, 
  BookOpen, 
  Users, 
  ArrowUpRight,
  PieChart,
  Smartphone,
  Lock,
  Layers,
  CheckCircle,
  Brain,
  X,
  Send,
  Sparkles,
  MessageSquare,
  LogOut,
  User,
  Mail,
  Loader2,
  Briefcase,
  Wallet,
  Target
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInWithPopup, 
  GoogleAuthProvider, 
  createUserWithEmailAndPassword, 
  signInWithEmailAndPassword, 
  signOut, 
  onAuthStateChanged,
  User as FirebaseUser,
  updateProfile,
  signInWithCustomToken, 
  signInAnonymously
} from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';

// --- Firebase Initialization ---
const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

// --- Helper: Text Formatting for AI ---
const formatMessageText = (text: string) => {
  const paragraphs = text.split(/\n\n+/);
  return paragraphs.map((paragraph, pIndex) => {
    if (paragraph.trim().startsWith('* ') || paragraph.trim().startsWith('- ')) {
      const items = paragraph.split(/\n/).filter(line => line.trim());
      return (
        <ul key={pIndex} className="list-disc pl-4 mb-3 space-y-1">
          {items.map((item, iIndex) => {
            const cleanItem = item.replace(/^[\*\-]\s/, '');
            const parts = cleanItem.split(/(\*\*.*?\*\*)/g);
            return (
              <li key={iIndex}>
                {parts.map((part, partIndex) => {
                  if (part.startsWith('**') && part.endsWith('**')) {
                    return <strong key={partIndex} className="text-blue-200 font-semibold">{part.slice(2, -2)}</strong>;
                  }
                  return part;
                })}
              </li>
            );
          })}
        </ul>
      );
    }
    const parts = paragraph.split(/(\*\*.*?\*\*)/g);
    return (
      <p key={pIndex} className="mb-3 last:mb-0">
        {parts.map((part, partIndex) => {
          if (part.startsWith('**') && part.endsWith('**')) {
            return <strong key={partIndex} className="text-blue-200 font-semibold">{part.slice(2, -2)}</strong>;
          }
          return part;
        })}
      </p>
    );
  });
};

// --- Components ---

const Navbar = ({ 
  onOpenMentor, 
  onOpenLogin, 
  user, 
  onLogout 
}: { 
  onOpenMentor: () => void, 
  onOpenLogin: () => void, 
  user: FirebaseUser | null, 
  onLogout: () => void 
}) => {
  const [scrolled, setScrolled] = useState(false);
  const [showProfileMenu, setShowProfileMenu] = useState(false);

  useEffect(() => {
    const handleScroll = () => setScrolled(window.scrollY > 20);
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  // Show user profile only if authenticated and not anonymous
  const showUser = user && !user.isAnonymous;

  return (
    <nav className={`fixed top-0 left-0 right-0 z-50 transition-all duration-300 ${
      scrolled ? 'bg-black/70 backdrop-blur-xl border-b border-white/10' : 'bg-transparent'
    }`}>
      <div className="max-w-7xl mx-auto px-6 h-12 flex items-center justify-between">
        <div className="flex items-center gap-2 cursor-pointer group">
          <div className="w-5 h-5 bg-white rounded-full flex items-center justify-center group-hover:bg-blue-500 transition-colors">
            <div className="w-2 h-2 bg-black rounded-full" />
          </div>
          <span className="font-semibold text-white tracking-tight text-sm">FinEdu</span>
        </div>

        <div className="hidden md:flex items-center gap-8 text-xs font-medium text-gray-300">
          <a href="#" className="hover:text-white transition-colors">The Mission</a>
          <button onClick={onOpenMentor} className="hover:text-white transition-colors">AI Mentor</button>
          <a href="#" className="hover:text-white transition-colors">Curriculum</a>
          <a href="#" className="hover:text-white transition-colors">Enterprise</a>
        </div>

        <div className="flex items-center gap-4">
          {showUser ? (
            <div className="relative">
              <button 
                onClick={() => setShowProfileMenu(!showProfileMenu)}
                className="flex items-center gap-2 hover:bg-white/10 p-1 pr-3 rounded-full transition-colors"
              >
                {user?.photoURL ? (
                  <img src={user.photoURL} alt="Profile" className="w-6 h-6 rounded-full border border-white/10" />
                ) : (
                  <div className="w-6 h-6 rounded-full bg-gradient-to-tr from-blue-500 to-purple-500 flex items-center justify-center text-[10px] font-bold text-white">
                    {user?.email?.[0].toUpperCase() || 'U'}
                  </div>
                )}
                <span className="text-xs text-white max-w-[100px] truncate">{user?.displayName || user?.email?.split('@')[0]}</span>
              </button>

              {showProfileMenu && (
                <div className="absolute right-0 top-full mt-2 w-48 bg-[#1c1c1e] border border-white/10 rounded-xl shadow-xl overflow-hidden py-1 animate-fade-in-up">
                  <div className="px-4 py-2 border-b border-white/5">
                    <p className="text-[10px] text-gray-500 uppercase tracking-wider">Account</p>
                    <p className="text-xs text-white truncate">{user?.email}</p>
                  </div>
                  <button className="w-full text-left px-4 py-2 text-xs text-gray-300 hover:bg-white/5 flex items-center gap-2">
                    <User size={12} /> Profile Settings
                  </button>
                  <button 
                    onClick={() => {
                      onLogout();
                      setShowProfileMenu(false);
                    }}
                    className="w-full text-left px-4 py-2 text-xs text-red-400 hover:bg-white/5 flex items-center gap-2"
                  >
                    <LogOut size={12} /> Log Out
                  </button>
                </div>
              )}
            </div>
          ) : (
            <>
              <button onClick={onOpenLogin} className="text-xs text-white hover:opacity-70 transition-opacity">Log in</button>
              <button onClick={onOpenLogin} className="bg-blue-600 text-white text-xs px-3 py-1.5 rounded-full hover:bg-blue-500 transition-colors font-medium">
                Start Learning
              </button>
            </>
          )}
        </div>
      </div>
    </nav>
  );
};

// --- Auth Modal ---
const AuthModal = ({ onClose }: { onClose: () => void }) => {
  const [isSignUp, setIsSignUp] = useState(false);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleGoogleLogin = async () => {
    setLoading(true);
    setError(null);
    try {
      const provider = new GoogleAuthProvider();
      await signInWithPopup(auth, provider);
      onClose();
    } catch (err: any) {
      console.error(err);
      setError("Failed to sign in with Google. Please try again.");
    } finally {
      setLoading(false);
    }
  };

  const handleEmailAuth = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    setError(null);

    try {
      if (isSignUp) {
        await createUserWithEmailAndPassword(auth, email, password);
      } else {
        await signInWithEmailAndPassword(auth, email, password);
      }
      onClose();
    } catch (err: any) {
      if (err.code === 'auth/email-already-in-use') {
        setError("That email is already registered. Try logging in.");
      } else if (err.code === 'auth/wrong-password' || err.code === 'auth/user-not-found' || err.code === 'auth/invalid-credential') {
        setError("Invalid email or password.");
      } else if (err.code === 'auth/weak-password') {
        setError("Password should be at least 6 characters.");
      } else {
        setError("Authentication failed. Please try again.");
      }
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="fixed inset-0 z-[70] flex items-center justify-center px-4">
      <div className="absolute inset-0 bg-black/60 backdrop-blur-md transition-opacity" onClick={onClose} />
      
      <div className="relative w-full max-w-md bg-[#1c1c1e] border border-white/10 rounded-2xl shadow-2xl overflow-hidden animate-fade-in-up">
        <div className="p-8">
          <div className="text-center mb-8">
            <div className="w-10 h-10 bg-white rounded-full flex items-center justify-center mx-auto mb-4">
              <div className="w-4 h-4 bg-black rounded-full" />
            </div>
            <h2 className="text-2xl font-bold text-white mb-2">{isSignUp ? 'Create your account' : 'Welcome back'}</h2>
            <p className="text-gray-400 text-sm">
              {isSignUp ? 'Start your journey to financial mastery.' : 'Enter your details to access your dashboard.'}
            </p>
          </div>

          {/* Google Button */}
          <button 
            onClick={handleGoogleLogin}
            disabled={loading}
            className="w-full bg-white text-black font-medium py-3 rounded-xl flex items-center justify-center gap-3 hover:bg-gray-100 transition-colors mb-6 disabled:opacity-70 disabled:cursor-not-allowed"
          >
            {loading ? <Loader2 className="animate-spin" size={20} /> : (
              <>
                <svg className="w-5 h-5" viewBox="0 0 24 24">
                  <path fill="currentColor" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" />
                  <path fill="currentColor" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" />
                  <path fill="currentColor" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" />
                  <path fill="currentColor" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" />
                </svg>
                <span>Continue with Google</span>
              </>
            )}
          </button>

          <div className="relative mb-6">
            <div className="absolute inset-0 flex items-center">
              <div className="w-full border-t border-white/10"></div>
            </div>
            <div className="relative flex justify-center text-xs uppercase">
              <span className="bg-[#1c1c1e] px-2 text-gray-500">Or continue with email</span>
            </div>
          </div>

          <form onSubmit={handleEmailAuth} className="space-y-4">
            {error && (
              <div className="p-3 rounded-lg bg-red-500/10 border border-red-500/20 text-red-400 text-xs text-center">
                {error}
              </div>
            )}
            
            <div className="space-y-1">
              <label className="text-xs font-medium text-gray-400 ml-1">Email</label>
              <div className="relative">
                <Mail className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-500" size={16} />
                <input 
                  type="email" 
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  required
                  className="w-full bg-[#2c2c2e] border border-white/5 rounded-xl py-3 pl-10 pr-4 text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500/50 text-sm"
                  placeholder="name@example.com"
                />
              </div>
            </div>

            <div className="space-y-1">
              <label className="text-xs font-medium text-gray-400 ml-1">Password</label>
              <div className="relative">
                <Lock className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-500" size={16} />
                <input 
                  type="password" 
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  required
                  className="w-full bg-[#2c2c2e] border border-white/5 rounded-xl py-3 pl-10 pr-4 text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500/50 text-sm"
                  placeholder="••••••••"
                />
              </div>
            </div>

            <button 
              type="submit" 
              disabled={loading}
              className="w-full bg-blue-600 text-white font-medium py-3 rounded-xl hover:bg-blue-500 transition-colors disabled:opacity-50 disabled:cursor-not-allowed mt-2"
            >
              {loading ? <Loader2 className="animate-spin mx-auto" size={20} /> : (isSignUp ? 'Create Account' : 'Log In')}
            </button>
          </form>

          <div className="mt-6 text-center">
            <p className="text-xs text-gray-400">
              {isSignUp ? "Already have an account?" : "Don't have an account?"}{' '}
              <button 
                onClick={() => { setIsSignUp(!isSignUp); setError(null); }}
                className="text-blue-400 hover:text-blue-300 font-medium"
              >
                {isSignUp ? 'Log in' : 'Sign up'}
              </button>
            </p>
          </div>
        </div>
      </div>
    </div>
  );
};

// --- AI Mentor Interface ---
const AIMentorInterface = ({ onClose, user }: { onClose: () => void, user: FirebaseUser | null }) => {
  const [messages, setMessages] = useState<{role: 'ai' | 'user', text: string}[]>([
    { role: 'ai', text: `Hello${user && !user.isAnonymous ? ' ' + (user.displayName || 'Friend').split(' ')[0] : ''}. I'm your FinEdu Mentor. I can help you plan your financial goals, explain complex terms, or analyze your portfolio. What's on your mind today?` }
  ]);
  const [inputValue, setInputValue] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const scrollRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (scrollRef.current) {
      scrollRef.current.scrollTop = scrollRef.current.scrollHeight;
    }
  }, [messages, isTyping]);

  const handleSend = async () => {
    if (!inputValue.trim()) return;
    const userText = inputValue;
    const updatedMessages = [...messages, { role: 'user' as const, text: userText }];
    setMessages(updatedMessages);
    setInputValue('');
    setIsTyping(true);

    try {
      const apiKey = ""; // The environment provides the key at runtime
      const apiContents = updatedMessages.map(msg => ({
        role: msg.role === 'ai' ? 'model' : 'user',
        parts: [{ text: msg.text }]
      }));

      const response = await fetch(
        `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`,
        {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            contents: apiContents,
            systemInstruction: {
              parts: [{ text: "You are the FinEdu AI Mentor. Your responses should be beautifully formatted, professional, and easy to read. Use **bold** for key concepts. Use bullet points for lists. Keep paragraphs short and spaced out. Your tone is 'Apple-style': clean, simple, and intelligent. Avoid long walls of text." }]
            }
          })
        }
      );
      const data = await response.json();
      const aiResponseText = data.candidates?.[0]?.content?.parts?.[0]?.text || "I apologize, but I'm unable to provide a response at the moment.";
      setMessages(prev => [...prev, { role: 'ai', text: aiResponseText }]);
    } catch (error) {
      setMessages(prev => [...prev, { role: 'ai', text: "I apologize, I'm having trouble connecting to the server. Please check your internet connection and try again." }]);
    } finally {
      setIsTyping(false);
    }
  };

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleSend();
    }
  };

  return (
    <div className="fixed inset-0 z-[60] flex items-center justify-center px-4">
      <div className="absolute inset-0 bg-black/60 backdrop-blur-md transition-opacity" onClick={onClose} />
      <div className="relative w-full max-w-2xl bg-[#1c1c1e] border border-white/10 rounded-2xl shadow-2xl flex flex-col h-[600px] overflow-hidden animate-fade-in-up">
        <div className="flex items-center justify-between p-4 border-b border-white/10 bg-[#1c1c1e]">
          <div className="flex items-center gap-3">
            <div className="w-8 h-8 rounded-full bg-blue-600 flex items-center justify-center">
              <Brain size={16} className="text-white" />
            </div>
            <div>
              <h3 className="text-white font-semibold text-sm">FinEdu Intelligence</h3>
              <div className="flex items-center gap-1.5">
                <span className="w-1.5 h-1.5 rounded-full bg-green-500 animate-pulse" />
                <span className="text-xs text-gray-400">Online & Learning</span>
              </div>
            </div>
          </div>
          <button onClick={onClose} className="w-8 h-8 rounded-full hover:bg-white/10 flex items-center justify-center text-gray-400 transition-colors">
            <X size={18} />
          </button>
        </div>
        <div className="flex-1 overflow-y-auto p-6 space-y-6" ref={scrollRef}>
          {messages.map((msg, idx) => (
            <div key={idx} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
              <div className={`max-w-[85%] rounded-2xl p-4 text-sm leading-relaxed ${msg.role === 'user' ? 'bg-blue-600 text-white rounded-br-none font-medium' : 'bg-[#2c2c2e] text-gray-100 rounded-bl-none border border-white/5'}`}>
                {msg.role === 'ai' ? formatMessageText(msg.text) : msg.text}
              </div>
            </div>
          ))}
          {isTyping && (
            <div className="flex justify-start">
              <div className="bg-[#2c2c2e] rounded-2xl rounded-bl-none p-4 border border-white/5 flex gap-1">
                <span className="w-2 h-2 bg-gray-500 rounded-full animate-bounce delay-0" />
                <span className="w-2 h-2 bg-gray-500 rounded-full animate-bounce delay-150" />
                <span className="w-2 h-2 bg-gray-500 rounded-full animate-bounce delay-300" />
              </div>
            </div>
          )}
        </div>
        <div className="p-4 bg-[#1c1c1e] border-t border-white/10">
          <div className="relative">
            <input type="text" value={inputValue} onChange={(e) => setInputValue(e.target.value)} onKeyDown={handleKeyDown} placeholder="Ask me anything about finance..." className="w-full bg-[#2c2c2e] text-white rounded-full pl-6 pr-12 py-4 focus:outline-none focus:ring-2 focus:ring-blue-500/50 border border-white/5 placeholder-gray-500 text-sm" />
            <button onClick={handleSend} disabled={!inputValue.trim()} className="absolute right-2 top-1/2 -translate-y-1/2 w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center text-white hover:bg-blue-500 disabled:opacity-50 disabled:cursor-not-allowed transition-all">
              <Send size={14} />
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

const Hero = ({ onOpenMentor, onOpenLogin }: { onOpenMentor: () => void, onOpenLogin: () => void }) => {
  return (
    <section className="relative min-h-screen flex flex-col items-center justify-center bg-black text-white pt-20 overflow-hidden">
      <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-blue-900/20 rounded-full blur-[120px] pointer-events-none" />
      <div className="z-10 flex flex-col items-center text-center px-4 max-w-4xl mx-auto animate-fade-in-up">
        <span className="text-blue-400 font-medium tracking-wide text-sm mb-6 border border-blue-400/30 px-3 py-1 rounded-full bg-blue-400/10 flex items-center gap-2">
          <Zap size={12} className="fill-current" /> The Future of Financial Literacy
        </span>
        <h1 className="text-5xl md:text-7xl lg:text-8xl font-bold tracking-tighter mb-6 bg-clip-text text-transparent bg-gradient-to-b from-white via-white to-gray-500">
          Financial education, <br /> finally simple.
        </h1>
        <p className="text-xl md:text-2xl text-gray-400 max-w-2xl font-light leading-relaxed mb-10">
          No noise. No confusion. No hidden agendas. 
          A full-stack platform built with the clarity of Apple and the intelligence of AI.
        </p>
        <div className="flex flex-col sm:flex-row gap-4 items-center">
          <button onClick={onOpenLogin} className="bg-white text-black text-lg px-8 py-3 rounded-full font-medium hover:scale-105 transition-transform flex items-center gap-2">
            Start Your Journey <ChevronRight size={18} />
          </button>
          <button onClick={onOpenMentor} className="text-blue-400 text-lg px-8 py-3 rounded-full font-medium hover:text-blue-300 transition-colors flex items-center gap-2 group border border-blue-400/30 hover:bg-blue-400/10">
            Meet Your AI Mentor <Brain size={18} className="group-hover:scale-110 transition-transform" />
          </button>
        </div>
      </div>
      <div className="mt-20 w-full max-w-6xl mx-auto relative z-10">
        <div className="bg-[#161617] rounded-t-3xl border border-white/10 p-2 md:p-4 shadow-2xl mx-4 md:mx-0">
          <div className="bg-black rounded-t-xl overflow-hidden aspect-[16/9] md:aspect-[21/9] relative flex items-center justify-center">
            <div className="absolute inset-0 bg-gradient-to-br from-gray-900 to-black p-8 grid grid-cols-12 gap-4 opacity-80">
               <div className="col-span-3 bg-white/5 rounded-xl h-full flex flex-col gap-3 p-4">
                  <div className="w-8 h-8 rounded-full bg-blue-600/50 mb-4" />
                  <div className="h-2 w-2/3 bg-white/10 rounded-full" />
                  <div className="h-2 w-1/2 bg-white/10 rounded-full" />
                  <div className="h-2 w-3/4 bg-white/10 rounded-full" />
               </div>
               <div className="col-span-9 flex flex-col gap-4">
                  <div className="h-1/4 bg-white/5 rounded-xl border border-white/5 p-6 flex items-center gap-4">
                     <div className="h-12 w-12 rounded-full bg-green-500/20 flex items-center justify-center text-green-400"><CheckCircle size={24} /></div>
                     <div>
                        <div className="h-3 w-32 bg-white/20 rounded-full mb-2" />
                        <div className="h-2 w-48 bg-white/10 rounded-full" />
                     </div>
                  </div>
                  <div className="h-3/4 bg-white/5 rounded-xl border border-white/5 relative overflow-hidden flex items-end">
                    <div className="w-full h-1/2 bg-gradient-to-t from-blue-500/10 to-transparent" />
                  </div>
               </div>
            </div>
            <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-center">
               <p className="text-white/40 font-mono text-sm mb-2">FinEdu Intelligent Dashboard</p>
               <span className="text-xs text-blue-500 bg-blue-500/10 px-2 py-1 rounded border border-blue-500/20">Live Preview</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  );
};

const TrustedBy = () => {
  return (
    <section className="py-12 bg-black border-b border-white/10 overflow-hidden relative">
      <style>{`
        @keyframes scroll {
          0% { transform: translateX(0); }
          100% { transform: translateX(-50%); }
        }
        .animate-scroll {
          animation: scroll 30s linear infinite;
        }
        .paused {
          animation-play-state: paused;
        }
      `}</style>
      <div className="text-center mb-8">
        <p className="text-sm font-medium text-gray-500 uppercase tracking-widest">Recognized as the gold standard by</p>
      </div>
      <div className="relative w-full max-w-[100vw] overflow-hidden group">
        <div className="absolute left-0 top-0 bottom-0 w-24 z-10 bg-gradient-to-r from-black to-transparent pointer-events-none"></div>
        <div className="absolute right-0 top-0 bottom-0 w-24 z-10 bg-gradient-to-l from-black to-transparent pointer-events-none"></div>
        <div className="flex items-center gap-16 whitespace-nowrap animate-scroll group-hover:paused w-max px-8">
           {/* Logo Set 1 */}
           <div className="flex items-center gap-16 opacity-50 grayscale hover:grayscale-0 transition-all duration-500">
              <span className="text-3xl font-serif font-bold tracking-tight text-white">Financial Times</span>
              <span className="text-2xl font-sans font-bold tracking-tighter text-white">Bloomberg</span>
              <span className="text-2xl font-serif font-bold text-orange-500">REUTERS</span>
              <span className="text-2xl font-serif font-bold italic text-white">THE WALL STREET JOURNAL.</span>
              <span className="text-3xl font-serif font-bold text-white">Forbes</span>
              <span className="text-2xl font-sans font-bold tracking-tight text-blue-400">CNBC</span>
              <span className="text-2xl font-serif font-bold bg-white text-black px-1">The Economist</span>
           </div>
           
           {/* Logo Set 2 (Duplicate) */}
           <div className="flex items-center gap-16 opacity-50 grayscale hover:grayscale-0 transition-all duration-500">
              <span className="text-3xl font-serif font-bold tracking-tight text-white">Financial Times</span>
              <span className="text-2xl font-sans font-bold tracking-tighter text-white">Bloomberg</span>
              <span className="text-2xl font-serif font-bold text-orange-500">REUTERS</span>
              <span className="text-2xl font-serif font-bold italic text-white">THE WALL STREET JOURNAL.</span>
              <span className="text-3xl font-serif font-bold text-white">Forbes</span>
              <span className="text-2xl font-sans font-bold tracking-tight text-blue-400">CNBC</span>
              <span className="text-2xl font-serif font-bold bg-white text-black px-1">The Economist</span>
           </div>
        </div>
      </div>
    </section>
  );
};

const CompoundingGraph = () => {
  const [monthlyContribution, setMonthlyContribution] = useState(500); // Default start value
  const [annualReturn, setAnnualReturn] = useState(8); // Default 8% return
  const [years, setYears] = useState(30); // Default 30 years
  
  // Formatting helper
  const formatCurrency = (val: number) => 
    new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(val);

  const width = 1000;
  const height = 400;
  const points = [];
  // Calculate dynamic monthly rate based on slider
  const monthlyRate = (annualReturn / 100) / 12; 
  
  // Calculate final value dynamically based on all states
  const maxVal = monthlyContribution * (Math.pow(1 + monthlyRate, years * 12) - 1) / monthlyRate;

  // Generate points
  // We use a fixed number of segments (e.g., 100) to keep the curve smooth regardless of year count
  const segments = 100;
  for (let i = 0; i <= segments; i++) { 
    const currentMonth = (i / segments) * (years * 12);
    const val = monthlyContribution * (Math.pow(1 + monthlyRate, currentMonth) - 1) / monthlyRate;
    
    const x = (i / segments) * width;
    // Normalize Y
    const y = height - ((val / (maxVal || 1)) * height * 0.8); 
    points.push(`${x},${y}`);
  }

  const pathData = `M0,${height} L` + points.join(' L');
  const areaPathData = pathData + ` L${width},${height} Z`;

  return (
    <section className="bg-black py-24 px-6 border-b border-white/10 overflow-hidden">
      <div className="max-w-7xl mx-auto">
        <div className="text-center mb-16">
           <h2 className="text-3xl md:text-5xl font-bold text-white mb-6">The Eighth Wonder of the World.</h2>
           <p className="text-xl text-gray-400 max-w-2xl mx-auto mb-10">
             Compound interest isn't magic; it's math. Use the sliders below to see how consistency, returns, and time impact your wealth.
           </p>
           
           {/* Interactive Slider Controls */}
           <div className="flex flex-col gap-6 bg-[#161617] p-6 rounded-2xl border border-white/10 max-w-xl mx-auto">
              
              {/* Contribution Slider */}
              <div className="flex flex-col gap-2">
                  <div className="flex justify-between w-full text-sm text-gray-400">
                    <span>Monthly Contribution</span>
                    <span className="text-white font-mono">{formatCurrency(monthlyContribution)}</span>
                  </div>
                  <input 
                    type="range" 
                    min="50" 
                    max="2500" 
                    step="50" 
                    value={monthlyContribution} 
                    onChange={(e) => setMonthlyContribution(Number(e.target.value))}
                    className="w-full h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-500/50"
                  />
                  <div className="flex justify-between w-full text-xs text-gray-500">
                    <span>$50</span>
                    <span>$2,500</span>
                  </div>
              </div>

              {/* Annual Return Slider */}
              <div className="flex flex-col gap-2">
                  <div className="flex justify-between w-full text-sm text-gray-400">
                    <span>Avg. Annual Return</span>
                    <span className="text-white font-mono">{annualReturn}%</span>
                  </div>
                  <input 
                    type="range" 
                    min="1" 
                    max="15" 
                    step="0.5" 
                    value={annualReturn} 
                    onChange={(e) => setAnnualReturn(Number(e.target.value))}
                    className="w-full h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-green-500 focus:outline-none focus:ring-2 focus:ring-green-500/50"
                  />
                  <div className="flex justify-between w-full text-xs text-gray-500">
                    <span>1%</span>
                    <span>15%</span>
                  </div>
              </div>

              {/* Years Slider */}
              <div className="flex flex-col gap-2">
                  <div className="flex justify-between w-full text-sm text-gray-400">
                    <span>Years to Grow</span>
                    <span className="text-white font-mono">{years} Years</span>
                  </div>
                  <input 
                    type="range" 
                    min="5" 
                    max="50" 
                    step="1" 
                    value={years} 
                    onChange={(e) => setYears(Number(e.target.value))}
                    className="w-full h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-purple-500 focus:outline-none focus:ring-2 focus:ring-purple-500/50"
                  />
                  <div className="flex justify-between w-full text-xs text-gray-500">
                    <span>5 Years</span>
                    <span>50 Years</span>
                  </div>
              </div>

           </div>
        </div>

        <div className="relative w-full aspect-[16/9] md:aspect-[21/9] bg-[#161617] rounded-3xl border border-white/5 p-8 overflow-hidden group">
           {/* Grid Lines */}
           <div className="absolute inset-0 grid grid-cols-6 grid-rows-4 border-white/5">
              {[...Array(24)].map((_, i) => (
                <div key={i} className="border-r border-b border-white/5" />
              ))}
           </div>

           {/* Labels */}
           <div className="absolute top-8 left-8 text-white/50 text-xs font-mono">{formatCurrency(maxVal)}</div>
           <div className="absolute bottom-8 left-8 text-white/50 text-xs font-mono">$0</div>
           <div className="absolute bottom-8 right-8 text-white/50 text-xs font-mono">{years} Years</div>

           {/* SVG Graph */}
           <svg className="absolute inset-0 w-full h-full p-8" viewBox={`0 0 ${width} ${height}`} preserveAspectRatio="none">
              <defs>
                <linearGradient id="graphGradient" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stopColor="#3b82f6" stopOpacity="0.4" />
                  <stop offset="100%" stopColor="#3b82f6" stopOpacity="0" />
                </linearGradient>
              </defs>

              <path d={areaPathData} fill="url(#graphGradient)" className="transition-all duration-300 ease-out" />

              <path 
                d={pathData} 
                fill="none" 
                stroke="#3b82f6" 
                strokeWidth="4" 
                strokeLinecap="round"
                className="drop-shadow-[0_0_10px_rgba(59,130,246,0.5)] transition-all duration-300 ease-out"
              />
              
              {/* Animated Dot at the end */}
              <circle cx={width} cy={height * 0.2} r="6" fill="white" className="animate-pulse shadow-[0_0_20px_rgba(255,255,255,0.8)]" />
           </svg>
           
           {/* Floating Badge */}
           <div className="absolute top-1/4 left-1/4 bg-black/80 backdrop-blur border border-white/10 p-4 rounded-xl shadow-2xl transform transition-transform hover:scale-105 duration-300 z-10">
              <div className="flex items-center gap-3 mb-1">
                 <div className="w-2 h-2 rounded-full bg-green-500 animate-pulse" />
                 <span className="text-gray-400 text-xs uppercase tracking-wider">Projected Wealth</span>
              </div>
              <div className="text-3xl font-bold text-white tracking-tight">{formatCurrency(maxVal)}</div>
              <div className="text-blue-400 text-xs mt-1">+{annualReturn}% Avg. Annual Return</div>
           </div>
        </div>
      </div>
    </section>
  )
}

const FinancialPillars = () => {
  return (
    <section className="bg-black py-32 px-6 border-b border-white/10">
      <div className="max-w-7xl mx-auto">
        <div className="text-center mb-20">
          <h2 className="text-3xl md:text-5xl font-bold text-white mb-6">The Financial Education Ladder.</h2>
          <p className="text-xl text-gray-400 max-w-2xl mx-auto">
            A structured path from stability to mastery.
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
           {/* Pillar 1: Personal Finance */}
           <div className="bg-[#161617] rounded-3xl p-8 border border-white/10 hover:border-blue-500/50 transition-colors group">
              <div className="w-12 h-12 bg-blue-500/10 rounded-xl flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                 <Wallet className="text-blue-400" size={24} />
              </div>
              <h3 className="text-2xl font-bold text-white mb-2">1. Personal Finance</h3>
              <span className="text-blue-400 text-xs font-bold tracking-wider uppercase mb-4 block">Foundation</span>
              <p className="text-gray-300 font-medium mb-4 italic">"Get control over your own money."</p>
              
              <ul className="space-y-3 mb-8">
                {['Budgeting & Cash Flow', 'Emergency Buffers', 'Debt & Credit Logic', 'Avoiding Lifestyle Inflation', 'Basic Decisions'].map(item => (
                    <li key={item} className="flex items-start gap-2 text-gray-400 text-sm">
                        <CheckCircle size={14} className="text-blue-500 mt-1 shrink-0"/> {item}
                    </li>
                ))}
              </ul>
              
              <div className="pt-6 border-t border-white/5">
                 <p className="text-xs text-gray-500 leading-relaxed">
                    <strong className="text-gray-300">Why it matters:</strong> You can’t build wealth on chaos. This stabilizes your life and creates your first layer of freedom.
                 </p>
              </div>
           </div>

           {/* Pillar 2: Investing */}
           <div className="bg-[#161617] rounded-3xl p-8 border border-white/10 hover:border-green-500/50 transition-colors group">
              <div className="w-12 h-12 bg-green-500/10 rounded-xl flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                 <TrendingUp className="text-green-400" size={24} />
              </div>
              <h3 className="text-2xl font-bold text-white mb-2">2. Investing</h3>
              <span className="text-green-400 text-xs font-bold tracking-wider uppercase mb-4 block">Growth</span>
              <p className="text-gray-300 font-medium mb-4 italic">"Make money work for you."</p>
              
              <ul className="space-y-3 mb-8">
                {['Stocks, Bonds & ETFs', 'Diversification & Risk', 'Retirement Planning', 'Evaluating Asset Quality', 'Insurance & Protection'].map(item => (
                    <li key={item} className="flex items-start gap-2 text-gray-400 text-sm">
                        <CheckCircle size={14} className="text-green-500 mt-1 shrink-0"/> {item}
                    </li>
                ))}
              </ul>
              
              <div className="pt-6 border-t border-white/5">
                 <p className="text-xs text-gray-500 leading-relaxed">
                    <strong className="text-gray-300">Why it matters:</strong> This stage transforms you from earning money to owning assets that earn for you.
                 </p>
              </div>
           </div>

           {/* Pillar 3: Capital Allocation */}
           <div className="bg-[#161617] rounded-3xl p-8 border border-white/10 hover:border-purple-500/50 transition-colors group">
              <div className="w-12 h-12 bg-purple-500/10 rounded-xl flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                 <Target className="text-purple-400" size={24} />
              </div>
              <h3 className="text-2xl font-bold text-white mb-2">3. Capital Strategy</h3>
              <span className="text-purple-400 text-xs font-bold tracking-wider uppercase mb-4 block">Mastery</span>
              <p className="text-gray-300 font-medium mb-4 italic">"Think like an investor."</p>
              
              <ul className="space-y-3 mb-8">
                {['Strategic Decision-Making', 'Opportunity Cost', 'Behavioral Finance', 'Business Valuation', 'Macro Cycles'].map(item => (
                    <li key={item} className="flex items-start gap-2 text-gray-400 text-sm">
                        <CheckCircle size={14} className="text-purple-500 mt-1 shrink-0"/> {item}
                    </li>
                ))}
              </ul>
              
              <div className="pt-6 border-t border-white/5">
                 <p className="text-xs text-gray-500 leading-relaxed">
                    <strong className="text-gray-300">Why it matters:</strong> This is where true independence happens—allocating capital with intention and skill.
                 </p>
              </div>
           </div>
        </div>
      </div>
    </section>
  )
}

const BentoGrid = ({ onOpenMentor }: { onOpenMentor: () => void }) => {
  return (
    <section className="bg-black py-32 px-6">
      <div className="max-w-7xl mx-auto">
        <div className="mb-20 text-center max-w-3xl mx-auto">
           <h2 className="text-4xl md:text-6xl font-bold text-white mb-6 tracking-tight">
             Mastery, decoded.
           </h2>
           <p className="text-xl text-gray-400">
             We stripped away the noise to build the financial education platform we wished we had. 
             Here is how we help you build wealth.
           </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-6 lg:grid-cols-12 gap-6">
          
          {/* Card 1: AI Mentor */}
          <div className="col-span-1 md:col-span-6 lg:col-span-8 row-span-2 bg-[#161617] rounded-[32px] p-8 md:p-12 border border-white/5 relative overflow-hidden group">
            <div className="relative z-10 flex flex-col justify-between h-full">
               <div>
                 <div className="flex items-center gap-2 mb-4">
                    <span className="w-2 h-2 rounded-full bg-blue-500 animate-pulse"/>
                    <span className="text-blue-400 font-medium text-xs tracking-wider uppercase">Live Guidance</span>
                 </div>
                 <h3 className="text-3xl md:text-4xl font-bold text-white mb-4">Your Personal<br/>Financial Co-Pilot.</h3>
                 <p className="text-gray-400 max-w-md leading-relaxed mb-6">
                   Stuck on a concept? Need to analyze a stock? Your AI mentor is available 24/7 to answer questions, explain jargon, and guide your learning path based on your goals.
                 </p>
                 <ul className="space-y-2 mb-8 text-gray-400 text-sm">
                    <li className="flex items-center gap-2"><CheckCircle size={14} className="text-blue-500"/> Personalized curriculum adjustments</li>
                    <li className="flex items-center gap-2"><CheckCircle size={14} className="text-blue-500"/> Instant portfolio feedback</li>
                    <li className="flex items-center gap-2"><CheckCircle size={14} className="text-blue-500"/> Jargon-free explanations</li>
                 </ul>
               </div>
               <button onClick={onOpenMentor} className="w-fit bg-white/10 hover:bg-white/20 text-white px-6 py-3 rounded-full text-sm font-medium transition-colors backdrop-blur-md border border-white/10 flex items-center gap-2 group-hover:scale-105 duration-300">
                  <Brain size={16} /> Chat with Mentor
               </button>
            </div>
            
            <div className="absolute right-0 top-1/2 -translate-y-1/2 w-[45%] h-[90%] border-l border-t border-b border-white/10 bg-black/40 rounded-l-2xl backdrop-blur-sm p-4 flex flex-col gap-3 transition-transform duration-500 translate-x-12 group-hover:translate-x-4">
               <div className="self-end bg-blue-600/20 text-blue-200 text-xs p-3 rounded-2xl rounded-br-none max-w-[90%] border border-blue-500/30">
                  How do I hedge against inflation?
               </div>
               <div className="self-start bg-[#2c2c2e] text-gray-300 text-xs p-3 rounded-2xl rounded-bl-none max-w-[90%] border border-white/5">
                  Great question. Common strategies include TIPS, commodities like Gold, or real estate (REITs). Let's dive into each...
               </div>
               <div className="self-end bg-blue-600/20 text-blue-200 text-xs p-3 rounded-2xl rounded-br-none max-w-[90%] border border-blue-500/30">
                  Show me some REIT examples.
               </div>
               <div className="absolute bottom-0 left-0 right-0 h-20 bg-gradient-to-t from-[#161617] to-transparent pointer-events-none"/>
            </div>
          </div>

          {/* Card 2: Simulator */}
          <div className="col-span-1 md:col-span-6 lg:col-span-4 bg-[#161617] rounded-[32px] p-8 border border-white/5 relative overflow-hidden group min-h-[350px]">
             <div className="relative z-10">
               <div className="w-12 h-12 bg-green-500/10 rounded-full flex items-center justify-center mb-6">
                 <TrendingUp className="text-green-400" size={24} />
               </div>
               <h3 className="text-2xl font-bold text-white mb-3">Risk-Free Trading.</h3>
               <p className="text-gray-400 text-sm leading-relaxed mb-6">
                 Practice with $100k in paper money. Experience the emotional highs and lows of the market without the financial risk.
               </p>
             </div>
             <div className="absolute bottom-0 left-0 right-0 h-32 bg-gradient-to-t from-green-500/10 to-transparent">
                <svg className="w-full h-full text-green-500/30" preserveAspectRatio="none" viewBox="0 0 100 50">
                   <path d="M0,50 L10,40 L20,45 L30,30 L40,35 L50,20 L60,25 L70,10 L80,15 L90,5 L100,0 V50 Z" fill="currentColor" />
                </svg>
             </div>
          </div>

          {/* Card 3: Certification */}
          <div className="col-span-1 md:col-span-6 lg:col-span-4 bg-[#161617] rounded-[32px] p-8 border border-white/5 relative overflow-hidden group min-h-[350px]">
             <div className="relative z-10">
               <div className="w-12 h-12 bg-purple-500/10 rounded-full flex items-center justify-center mb-6">
                 <Shield className="text-purple-400" size={24} />
               </div>
               <h3 className="text-2xl font-bold text-white mb-3">Industry Certified.</h3>
               <p className="text-gray-400 text-sm leading-relaxed">
                 Earn certificates recognized by top fintech firms. Prove your mastery in Technical Analysis, DeFi, and Macroeconomics.
               </p>
             </div>
             <div className="absolute bottom-[-20px] right-[-20px] opacity-20 group-hover:opacity-40 transition-opacity rotate-12">
               <Shield size={180} className="text-purple-500" />
             </div>
          </div>

          {/* Card 4: Stats */}
          <div className="col-span-1 md:col-span-12 bg-gradient-to-r from-[#1c1c1e] to-[#0d0d0d] rounded-[32px] p-8 md:p-12 border border-white/5 flex flex-col md:flex-row items-center justify-between gap-10 group">
            <div className="flex-1">
               <span className="text-orange-400 font-medium text-xs tracking-wider uppercase mb-2 block">Our Impact</span>
               <h3 className="text-3xl font-bold text-white mb-4">Trusted by 50,000+ Investors.</h3>
               <p className="text-gray-400 max-w-lg mb-8">
                 From college students building their first portfolio to retirees managing their estate, FinEdu provides the clarity needed for every stage of wealth.
               </p>
               <div className="flex gap-8">
                  <div>
                     <p className="text-3xl font-bold text-white">4.9/5</p>
                     <p className="text-xs text-gray-500 mt-1 uppercase tracking-wide">App Store Rating</p>
                  </div>
                  <div>
                     <p className="text-3xl font-bold text-white">92%</p>
                     <p className="text-xs text-gray-500 mt-1 uppercase tracking-wide">Completion Rate</p>
                  </div>
                  <div>
                     <p className="text-3xl font-bold text-white">$200M+</p>
                     <p className="text-xs text-gray-500 mt-1 uppercase tracking-wide">Paper Money Traded</p>
                  </div>
               </div>
            </div>
            
            <div className="relative w-full md:w-1/3 h-48 bg-white/5 rounded-2xl border border-white/5 overflow-hidden flex items-center justify-center">
               <div className="absolute inset-0 flex flex-wrap gap-4 p-4 opacity-50">
                  {[...Array(12)].map((_, i) => (
                    <div key={i} className="w-12 h-12 rounded-full bg-white/10 animate-pulse" style={{animationDelay: `${i * 0.1}s`}} />
                  ))}
               </div>
               <div className="z-10 bg-black/80 backdrop-blur px-6 py-3 rounded-full border border-white/10 flex items-center gap-3">
                  <div className="flex -space-x-3">
                     <div className="w-8 h-8 rounded-full bg-blue-500 border-2 border-black"/>
                     <div className="w-8 h-8 rounded-full bg-purple-500 border-2 border-black"/>
                     <div className="w-8 h-8 rounded-full bg-green-500 border-2 border-black"/>
                  </div>
                  <span className="text-xs font-bold text-white">Join the community</span>
               </div>
            </div>
          </div>

        </div>
      </div>
    </section>
  );
};

const CourseCard = ({ title, category, duration, color }: { title: string, category: string, duration: string, color: string }) => (
  <div className="min-w-[300px] md:min-w-[400px] h-[500px] bg-[#1d1d1f] rounded-[24px] p-8 flex flex-col justify-between relative overflow-hidden hover:scale-[1.02] transition-transform duration-500 cursor-pointer snap-center">
    <div className="z-10 relative">
      <span className="text-xs font-semibold tracking-wider text-gray-400 uppercase mb-2 block">{category}</span>
      <h3 className="text-3xl font-bold text-white leading-tight">{title}</h3>
    </div>
    <div className="z-10 relative">
      <div className="flex items-center gap-3 text-white mb-6">
         <div className="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">
            <Play size={12} className="ml-1 fill-white" />
         </div>
         <span className="text-sm font-medium">{duration}</span>
      </div>
      <button className="w-full py-3 rounded-full bg-white text-black font-semibold text-sm hover:bg-gray-200 transition-colors">
        Start Module
      </button>
    </div>
    <div className={`absolute top-0 right-0 w-[400px] h-[400px] bg-${color}-500/20 blur-[80px] rounded-full translate-x-1/2 -translate-y-1/2`} />
  </div>
);

const Carousel = () => {
  return (
    <section className="bg-[#101010] py-32 overflow-hidden">
      <div className="max-w-7xl mx-auto px-6 mb-12 flex items-end justify-between">
         <div>
            <h2 className="text-4xl md:text-5xl font-bold text-white mb-4">Zero to Mastery.</h2>
            <p className="text-gray-400 text-lg">Curated paths tailored to your knowledge level.</p>
         </div>
         <div className="hidden md:flex gap-2">
            <button className="w-10 h-10 rounded-full bg-[#2d2d2f] flex items-center justify-center text-white hover:bg-[#3d3d3f]"><ChevronRight className="rotate-180" size={20}/></button>
            <button className="w-10 h-10 rounded-full bg-[#2d2d2f] flex items-center justify-center text-white hover:bg-[#3d3d3f]"><ChevronRight size={20}/></button>
         </div>
      </div>
      <div className="flex overflow-x-auto gap-6 px-6 pb-12 snap-x scrollbar-hide">
        <div className="w-0 md:w-[calc(50vw-42rem)] flex-shrink-0" />
        <CourseCard title="Financial Foundations" category="Level 1: Beginner" duration="4h 15m" color="green" />
        <CourseCard title="Investing 101" category="Level 2: Intermediate" duration="6h 30m" color="blue" />
        <CourseCard title="Stock Analysis & Valuation" category="Level 3: Advanced" duration="10h 00m" color="purple" />
        <CourseCard title="Retirement & Estate Planning" category="Level 4: Wealth" duration="5h 45m" color="orange" />
        <CourseCard title="Market Psychology" category="Elective" duration="3h 20m" color="pink" />
        <div className="w-6 flex-shrink-0" />
      </div>
    </section>
  );
};

const FeaturesList = () => {
  return (
    <section className="bg-black py-32 px-6">
       <div className="max-w-4xl mx-auto">
          <div className="grid gap-20">
             <div className="flex flex-col md:flex-row gap-10 items-center border-b border-white/10 pb-20">
                <div className="flex-1">
                   <h3 className="text-3xl md:text-4xl font-bold text-white mb-4">Clarity over Noise.</h3>
                   <p className="text-gray-400 text-lg leading-relaxed">
                      The internet is a maze of outdated courses and influencers who oversimplify. FinEdu is the antidote. We guide you from "I know nothing" to confident decision-making without the overwhelm.
                   </p>
                </div>
                <div className="flex-1 flex justify-center">
                   <Shield size={100} className="text-gray-700" strokeWidth={0.5} />
                </div>
             </div>
             <div className="flex flex-col md:flex-row-reverse gap-10 items-center border-b border-white/10 pb-20">
                <div className="flex-1">
                   <h3 className="text-3xl md:text-4xl font-bold text-white mb-4">Accessible Intelligence.</h3>
                   <p className="text-gray-400 text-lg leading-relaxed">
                      We combined the bite-sized accessibility of apps like Duolingo with the depth of a university degree. It's finance, designed for humans, powered by an AI that knows exactly what you need next.
                   </p>
                </div>
                <div className="flex-1 flex justify-center">
                   <Smartphone size={100} className="text-gray-700" strokeWidth={0.5} />
                </div>
             </div>
             <div className="flex flex-col md:flex-row gap-10 items-center">
                <div className="flex-1">
                   <h3 className="text-3xl md:text-4xl font-bold text-white mb-4">Full Stack Ecosystem.</h3>
                   <p className="text-gray-400 text-lg leading-relaxed">
                      Learning, researching, planning, and community—all in one place. Don't just watch videos. Use real market data to build projects and simulate decisions before risking a dime.
                   </p>
                </div>
                <div className="flex-1 flex justify-center">
                   <Layers size={100} className="text-gray-700" strokeWidth={0.5} />
                </div>
             </div>
          </div>
       </div>
    </section>
  )
}

const CTA = ({ onOpenLogin }: { onOpenLogin: () => void }) => {
  return (
    <section className="bg-[#161617] py-32 px-6 text-center">
      <div className="max-w-3xl mx-auto">
        <h2 className="text-5xl md:text-7xl font-bold text-white mb-8 tracking-tighter">
          Universal Financial Literacy.
        </h2>
        <p className="text-xl text-gray-400 mb-10">
          Join the platform that is empowering a generation to understand money, build wealth, and make confident decisions.
        </p>
        <div className="flex flex-col sm:flex-row justify-center gap-4">
          <button onClick={onOpenLogin} className="bg-blue-600 text-white text-lg px-8 py-4 rounded-full font-medium hover:bg-blue-500 transition-colors">
            Get Started Free
          </button>
          <button className="text-blue-400 text-lg px-8 py-4 rounded-full font-medium hover:text-blue-300 transition-colors">
            View Enterprise Solutions
          </button>
        </div>
        <p className="mt-8 text-xs text-gray-500">
          No credit card required for introductory paths.
        </p>
      </div>
    </section>
  );
};

const Footer = () => {
  return (
    <footer className="bg-black text-gray-400 py-12 px-6 text-xs border-t border-white/10">
      <div className="max-w-7xl mx-auto grid grid-cols-2 md:grid-cols-5 gap-8 mb-12">
        <div className="col-span-2">
          <div className="flex items-center gap-2 mb-4 text-white">
            <div className="w-4 h-4 bg-white rounded-full" />
            <span className="font-semibold">FinEdu</span>
          </div>
          <p className="max-w-xs">The world's most trusted financial learning system. Built for clarity, powered by intelligence.</p>
        </div>
        <div>
          <h4 className="text-white font-medium mb-4">Learn</h4>
          <ul className="space-y-2">
            <li><a href="#" className="hover:underline">Foundations</a></li>
            <li><a href="#" className="hover:underline">Investing</a></li>
            <li><a href="#" className="hover:underline">Market Analysis</a></li>
          </ul>
        </div>
        <div>
          <h4 className="text-white font-medium mb-4">Ecosystem</h4>
          <ul className="space-y-2">
            <li><a href="#" className="hover:underline">AI Mentor</a></li>
            <li><a href="#" className="hover:underline">Simulator</a></li>
            <li><a href="#" className="hover:underline">Enterprise</a></li>
          </ul>
        </div>
        <div>
          <h4 className="text-white font-medium mb-4">Company</h4>
          <ul className="space-y-2">
            <li><a href="#" className="hover:underline">Our Mission</a></li>
            <li><a href="#" className="hover:underline">Careers</a></li>
            <li><a href="#" className="hover:underline">Legal</a></li>
          </ul>
        </div>
      </div>
      <div className="max-w-7xl mx-auto pt-8 border-t border-white/10 flex flex-col md:flex-row justify-between items-center gap-4">
        <p>Copyright © 2024 FinEdu Inc. All rights reserved.</p>
        <div className="flex gap-4">
          <a href="#" className="hover:text-white">Privacy Policy</a>
          <a href="#" className="hover:text-white">Terms of Use</a>
          <a href="#" className="hover:text-white">Sales Policy</a>
        </div>
      </div>
    </footer>
  );
};

export default function App() {
  const [showMentor, setShowMentor] = useState(false);
  const [showLogin, setShowLogin] = useState(false);
  const [user, setUser] = useState<FirebaseUser | null>(null);

  // Monitor Auth State
  useEffect(() => {
    // 1. Initialize anonymous auth for potential db access, or custom token if available
    const initAuth = async () => {
      if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
        await signInWithCustomToken(auth, __initial_auth_token);
      } else {
        // We do NOT auto-sign in anonymously here to preserve the "Log In" UI state for the user
        // If we needed public data access immediately, we would sign in anonymously 
        // but hide the user state in the UI. 
        // For this demo, explicit login is preferred.
        await signInAnonymously(auth);
      }
    };
    initAuth();

    // 2. Listen for auth changes
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
      if (currentUser && !currentUser.isAnonymous) {
        setShowLogin(false); // Close login modal on successful standard auth
      }
    });
    return () => unsubscribe();
  }, []);

  const handleLogout = async () => {
    try {
      await signOut(auth);
    } catch (error) {
      console.error("Logout error", error);
    }
  };

  return (
    <div className="font-sans antialiased bg-black min-h-screen text-gray-100 selection:bg-blue-500/30 selection:text-blue-200">
      <Navbar 
        onOpenMentor={() => setShowMentor(true)} 
        onOpenLogin={() => setShowLogin(true)}
        user={user}
        onLogout={handleLogout}
      />
      
      <Hero 
        onOpenMentor={() => setShowMentor(true)} 
        onOpenLogin={() => setShowLogin(true)} 
      />
      
      <TrustedBy />

      <CompoundingGraph />

      <FinancialPillars />

      <BentoGrid onOpenMentor={() => setShowMentor(true)} />
      <Carousel />
      <FeaturesList />
      
      <CTA onOpenLogin={() => setShowLogin(true)} />
      
      <Footer />
      
      {/* AI Mentor Modal */}
      {showMentor && <AIMentorInterface onClose={() => setShowMentor(false)} user={user} />}

      {/* Login Modal */}
      {showLogin && <AuthModal onClose={() => setShowLogin(false)} />}
    </div>
  );
}
