import React, { useState } from 'react';
import { ArrowRight, ArrowLeft, Check, User, Target, Zap, MessageCircle, Link, Mic, Users, Heart, Trophy, Calendar, FileText, Download, Copy, RefreshCw, TrendingUp, Lightbulb, Camera, Star, Edit3, Upload } from 'lucide-react';

const ThreadsContentSystem = () => {
  const [currentView, setCurrentView] = useState('onboarding');
  const [currentStep, setCurrentStep] = useState(0);
  const [userProfile, setUserProfile] = useState({
    industry: '',
    idealClient: '',
    transformation: '',
    uniqueEdge: '',
    bestContent: '',
    trafficDestination: '',
    voiceStyle: '',
    clientTestimonials: '',
    personalityThemes: [],
    // Conditional personal details
    enneagramType: '',
    parentingStyle: '',
    kidAges: '',
    cookingStyle: '',
    fitnessStyle: '',
    faithBackground: '',
    travelStyle: '',
    entrepreneurStory: '',
    popCultureVibe: '',
    sportsBackground: '',
    musicTaste: ''
  });

  const [selectedWeek, setSelectedWeek] = useState(1);
  const [batchContent, setBatchContent] = useState([]);
  const [isGenerating, setIsGenerating] = useState(false);
  const [uploadError, setUploadError] = useState('');
  const [generationProgress, setGenerationProgress] = useState([]);
  const [showCalendarTooltip, setShowCalendarTooltip] = useState(false);
  const [showExportTooltip, setShowExportTooltip] = useState(false);
  const [selectedCalendarDay, setSelectedCalendarDay] = useState(null);

  // Add copy to clipboard function
  const copyToClipboard = (text) => {
    if (navigator.clipboard && window.isSecureContext) {
      navigator.clipboard.writeText(text).then(() => {
        // Could add a toast notification here
      }).catch(err => {
        console.error('Failed to copy: ', err);
        // Fallback to the older method
        fallbackCopyTextToClipboard(text);
      });
    } else {
      // Fallback for older browsers or non-secure contexts
      fallbackCopyTextToClipboard(text);
    }
  };

  const fallbackCopyTextToClipboard = (text) => {
    const textArea = document.createElement("textarea");
    textArea.value = text;
    
    // Avoid scrolling to bottom
    textArea.style.top = "0";
    textArea.style.left = "0";
    textArea.style.position = "fixed";

    document.body.appendChild(textArea);
    textArea.focus();
    textArea.select();

    try {
      const successful = document.execCommand('copy');
      if (!successful) throw new Error('Copy command was unsuccessful');
    } catch (err) {
      console.error('Fallback: Oops, unable to copy', err);
    }

    document.body.removeChild(textArea);
  };

  const voiceOptions = [
    { id: 'bold', label: 'Bold & Direct', icon: Zap, desc: 'Confident, no-nonsense, cuts through the noise' },
    { id: 'snarky', label: 'Snarky & Smart', icon: MessageCircle, desc: 'Witty, clever, calls out industry BS' },
    { id: 'relatable', label: 'Relatable & Real', icon: Heart, desc: 'Authentic, vulnerable, shares struggles' },
    { id: 'professional', label: 'Professional & Polished', icon: Users, desc: 'Authoritative, data-driven, credible' }
  ];

  const personalityThemes = [
    'Enneagram insights', 'Parenting/family life', 'Millennial experiences', 'Pop culture references',
    'Faith/spirituality', 'Sports analogies', 'Fitness/wellness', 'Travel stories',
    'Entrepreneurship journey', 'Tech/digital trends', 'Food/cooking', 'Music references'
  ];

  const weeklyFramework = {
    1: {
      title: "Foundation Building",
      subtitle: "Establish authority and challenge misconceptions",
      days: {
        1: { type: "Strategic Promo", icon: Link, color: "bg-blue-500" },
        2: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        3: { type: "Client Experience", icon: Users, color: "bg-green-500" },
        4: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        5: { type: "Client Experience", icon: Users, color: "bg-green-500" },
        6: { type: "Funny/Relatable", icon: Heart, color: "bg-yellow-500" },
        7: { type: "Industry Observation", icon: TrendingUp, color: "bg-purple-500" }
      }
    },
    2: {
      title: "Authority Establishment",
      subtitle: "Show results + behind the scenes",
      days: {
        8: { type: "Strategic Promo", icon: Link, color: "bg-blue-500" },
        9: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        10: { type: "Client Results", icon: Trophy, color: "bg-green-500" },
        11: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        12: { type: "Client Testimonials", icon: Star, color: "bg-pink-500" },
        13: { type: "Memes/Funny", icon: Heart, color: "bg-yellow-500" },
        14: { type: "Industry Observation", icon: TrendingUp, color: "bg-purple-500" }
      }
    },
    3: {
      title: "Community Building",
      subtitle: "Get personal and engage",
      days: {
        15: { type: "Strategic Promo", icon: Link, color: "bg-blue-500" },
        16: { type: "Engagement Thread", icon: MessageCircle, color: "bg-indigo-500" },
        17: { type: "Client Experience", icon: Users, color: "bg-green-500" },
        18: { type: "Engagement Thread", icon: MessageCircle, color: "bg-indigo-500" },
        19: { type: "Client Experience", icon: Users, color: "bg-green-500" },
        20: { type: "Funny/Relatable", icon: Heart, color: "bg-yellow-500" },
        21: { type: "Power Statement", icon: Zap, color: "bg-red-500" }
      }
    },
    4: {
      title: "Thought Leadership",
      subtitle: "Call out problems + show future thinking",
      days: {
        22: { type: "Strategic Promo", icon: Link, color: "bg-blue-500" },
        23: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        24: { type: "Industry Callout", icon: TrendingUp, color: "bg-purple-500" },
        25: { type: "Client Testimonials", icon: Star, color: "bg-pink-500" },
        26: { type: "Industry Callout", icon: TrendingUp, color: "bg-purple-500" },
        27: { type: "Memes/Funny", icon: Heart, color: "bg-yellow-500" },
        28: { type: "Industry Callout", icon: TrendingUp, color: "bg-purple-500" },
        29: { type: "Power Statement", icon: Zap, color: "bg-red-500" },
        30: { type: "Wrap-up Experience", icon: Star, color: "bg-amber-500" }
      }
    }
  };

  const questions = [
    {
      title: "What's Your Industry?",
      subtitle: "This helps us understand your market and audience",
      icon: User,
      field: 'industry',
      type: 'text',
      placeholder: 'e.g., Digital marketing, Real estate, Coaching, SaaS'
    },
    {
      title: "Who's Your Ideal Client?",
      subtitle: "Be specific - this shapes every piece of content",
      icon: Target,
      field: 'idealClient',
      type: 'textarea',
      placeholder: 'e.g., Small business owners making $100K-$500K who struggle with systems and want to scale without burnout'
    },
    {
      title: "What Transformation Do You Deliver?",
      subtitle: "The before → after result your clients achieve",
      icon: Trophy,
      field: 'transformation',
      type: 'textarea',
      placeholder: 'e.g., Help overwhelmed entrepreneurs build automated systems so they can scale to 7-figures while working fewer hours'
    },
    {
      title: "What's Your Unique Edge?",
      subtitle: "Your 'special sauce' - what makes you different",
      icon: Zap,
      field: 'uniqueEdge',
      type: 'textarea',
      placeholder: 'e.g., My 90-day rapid implementation method that gets results 3x faster than traditional approaches'
    },
    {
      title: "Share Your Best Performing Content",
      subtitle: "Paste 3 of your highest-performing social posts or emails",
      icon: TrendingUp,
      field: 'bestContent',
      type: 'textarea',
      placeholder: 'Paste your 3 best performing posts/emails here. Include engagement numbers if you have them (likes, comments, opens, clicks). This helps us nail your winning voice.'
    },
    {
      title: "What Voice Should We Aim For?",
      subtitle: "Based on your best content, how should your threads sound?",
      icon: Mic,
      field: 'voiceStyle',
      type: 'radio'
    },
    {
      title: "Client Testimonials & Results",
      subtitle: "Share 4 different client testimonials we can use in threads",
      icon: Star,
      field: 'clientTestimonials',
      type: 'textarea',
      placeholder: 'Share 4 client testimonials/results. Include: client name (first name or initials), their specific result, and what they said. Example: "Sarah K. - Went from $3K to $15K months in 90 days. Her words: \'I can\'t believe how fast this worked. I\'m booked out for 3 months!\'"'
    },
    {
      title: "Primary Traffic Destination",
      subtitle: "Where do you want to drive people?",
      icon: Link,
      field: 'trafficDestination',
      type: 'text',
      placeholder: 'e.g., Lead magnet signup, podcast, consultation booking, course page'
    },
    {
      title: "Your Personality Themes",
      subtitle: "What personal elements make your content uniquely you?",
      icon: Heart,
      field: 'personalityThemes',
      type: 'checkbox'
    },
    {
      title: "Client Success Stories",
      subtitle: "Share 1-2 specific client results we can reference",
      icon: Trophy,
      field: 'clientStories',
      type: 'textarea',
      placeholder: 'e.g., Sarah went from $50K to $200K in 8 months using my system. Mike automated 80% of his processes and took a 3-week vacation.'
    }
  ];

  const generateThread = async (type, day) => {
    // Use Claude API for all weeks (days 1-30)
    if (day >= 1 && day <= 30) {
      return await generateThreadWithClaude(type, day);
    }
    
    // Fallback for any edge cases (shouldn't be needed with days 1-30)
    return generateThreadFallback(type, day);
  };

  const generateThreadWithClaude = async (type, day) => {
    try {
      let templateAndStrategy = "";
      
      // Define specific templates for each day of Weeks 1-3
      if (day >= 1 && day <= 7) {
        switch (day) {
          case 1:
            templateAndStrategy = `Day 1: Challenge Kickoff Thread - Template: "30-Day Challenge Announcement"
            Starting a 30-day Threads challenge today to [YOUR GOAL/FOCUS].
            You're about to see a lot more of me over the next month.
            [WHAT THEY CAN EXPECT] coming your way.
            Prepare to be sick of me! 😜`;
            break;
          case 2:
            templateAndStrategy = `Day 2: Power Statement Thread - Template: "Don't Ask Me"
            Don't ask me for [YOUR TYPE OF] advice or you'll end up [POSITIVE RESULT FROM WORKING WITH YOU].`;
            break;
          case 3:
            templateAndStrategy = `Day 3: Experience/Client Thread - Template: "After [X], Here's What I Learned"
            After [WORKING WITH X CLIENTS/YEARS IN BUSINESS/MAJOR EXPERIENCE], here's what I wish I knew on day one:
            [KEY INSIGHT THAT CHALLENGES COMMON BELIEF]
            I used to [OLD WAY OF THINKING/DOING].
            Now I [NEW APPROACH - 1 sentence].
            Everything else is just [METAPHOR FOR SUPERFICIAL STUFF].`;
            break;
          case 4:
            templateAndStrategy = `Day 4: Power Statement Thread - Template: "POV"
            POV: You're a [TARGET MARKET IDENTIFIER] tired of [SPECIFIC PAIN POINT] and find my threads account.👀
            Suddenly you realize [YOUR SOLUTION/APPROACH] isn't [PERCEIVED DIFFICULTY] - you just needed [WHAT YOU PROVIDE].`;
            break;
          case 5:
            templateAndStrategy = `Day 5: Experience/Client Thread - Template: "The [TYPE] Energy"
            [YOUR PERSONALITY TYPE/IDENTIFIER] energy: [RELATABLE BEHAVIOR/THOUGHT PATTERN].
            Not out of [NEGATIVE ASSUMPTION]. Out of [POSITIVE TRUTH].
            We don't [WHAT OTHERS THINK YOU DO] - we [WHAT YOU ACTUALLY DO].`;
            break;
          case 6:
            templateAndStrategy = `Day 6: Funny/Relatable Thread - Template: "What's Better"
            What's better: [COMMON EXPENSIVE/WASTEFUL THING] or [IDEAL RESULT YOUR CLIENT WANTS TO ACHIEVE]?
            [LIST 3-4 SPECIFIC WINS YOUR CLIENTS GET]`;
            break;
          case 7:
            templateAndStrategy = `Day 7: Industry Observation Thread - Template: "My Dream [CLIENT/STUDENT] Is Someone Who"
            My dream [CLIENT/STUDENT] is someone who [POSITIVE TRAIT/BEHAVIOR 1].
            Someone who [POSITIVE TRAIT/BEHAVIOR 2] and [POSITIVE TRAIT/BEHAVIOR 3].
            If this sounds like you, we're going to work beautifully together.`;
            break;
        }
      } else if (day >= 8 && day <= 14) {
        // Week 2: Authority Establishment templates
        switch (day) {
          case 8:
            templateAndStrategy = `Day 8: Strategic Promotional Thread - Template: "You Don't Need Another"
            You don't need another [COMMON SOLUTION/COURSE/TOOL IN YOUR INDUSTRY].
            You need [SPECIFIC TYPE OF SUPPORT/EXPERTISE] when [URGENT SCENARIO/PAIN POINT].
            That's what [YOUR PRODUCT/SERVICE] provides - [SPECIFIC BENEFIT] when you need it most.
            Because [INDUSTRY/PLATFORM/SYSTEM] does not make this easy.
            Get the [BENEFIT DESCRIPTOR] you need & [ACTION] today → [LINK]`;
            break;
          case 9:
            templateAndStrategy = `Day 9: Experience/Client Thread - Template: "While Everyone Else Scrambles"
            [INDUSTRY CHANGE/CHALLENGE] happens. Panic sets in.
            While everyone else scrambles for [TYPICAL SOLUTION], my [CLIENTS/STUDENTS] get [YOUR UNIQUE SOLUTION].
            That's the difference between [WHAT COMPETITORS OFFER] and [WHAT YOU PROVIDE].`;
            break;
          case 10:
            templateAndStrategy = `Day 10: Power Statement Thread - Template: "If You Can't [X], You Don't Have [Y]"
            If you can't [ESSENTIAL BUSINESS SKILL], you don't have a business.
            You have [METAPHOR FOR WHAT THEY ACTUALLY HAVE].
            Real businesses [WHAT SUCCESSFUL BUSINESSES DO]. Even when [RELATABLE SCENARIO].`;
            break;
          case 11:
            templateAndStrategy = `Day 11: Experience/Client Thread - Template: "Remember [NOSTALGIC REFERENCE]?"
            Remember [NOSTALGIC CHILDHOOD/PAST REFERENCE]?
            That's the exact [QUALITY/ATTENTION] your [BUSINESS/GOAL] needs in the first [TIME PERIOD].
            [NEGLECT SCENARIO]? It [FAILS/DIES]. [PROPER ATTENTION SCENARIO]? It [THRIVES/SUCCEEDS].`;
            break;
          case 12:
            templateAndStrategy = `Day 12: Power Statement Thread - Template: "You Look Happier" Variation
            "you look [POSITIVE CHANGE]"
            Thanks I stopped [TOXIC INDUSTRY BEHAVIOR/MINDSET].
            Now I [YOUR BETTER APPROACH/SYSTEM].`;
            break;
          case 13:
            templateAndStrategy = `Day 13: Funny/Relatable Thread - Template: "I'm The Type"
            I'm the type that can't [SIMPLE ACTIVITY] without also [PRODUCTIVE ACTIVITY 1], [PRODUCTIVE ACTIVITY 2], and [PRODUCTIVE ACTIVITY 3].
            [YOUR PERSONALITY TYPE/TRAIT]s don't [RELAX/SIMPLE THING] - we [WHAT YOU ACTUALLY DO].
            [RELATABLE CONFESSION ABOUT YOUR TYPE]. ✨`;
            break;
          case 14:
            templateAndStrategy = `Day 14: Industry Observation Thread - Template: "Reality Check"
            Here's your reality check: Look at your last [NUMBER] [ATTEMPTS/LAUNCHES/EFFORTS].
            [DIAGNOSTIC QUESTION THAT REVEALS THE REAL PROBLEM]?
            If it's [SURFACE-LEVEL ANSWER], you don't have a [ASSUMED PROBLEM] problem.
            You have a [REAL PROBLEM] problem.`;
            break;
        }
      } else if (day >= 15 && day <= 21) {
        // Week 3: Community Building templates
        switch (day) {
          case 15:
            templateAndStrategy = `Day 15: Strategic Promotional Thread - Template: "Most [COMPETITORS] vs. What I Do"
            Most [COMPETITORS/OTHERS IN YOUR INDUSTRY] give you [BASIC OFFERING] and disappear.
            [YOUR BUSINESS] gives you [COMPREHENSIVE OFFERING] plus [ONGOING SUPPORT].
            Because let's be honest - you need more than [BASIC THING] to [ACHIEVE GOAL].
            [Link to valuable content/resource]`;
            break;
          case 16:
            templateAndStrategy = `Day 16: Engagement Thread - Template: "Wanna Know What's [ADJECTIVE]?"
            Wanna know what's [POSITIVE ADJECTIVE] about [PLATFORM/COMMUNITY/EXPERIENCE]?
            [SURPRISING POSITIVE OUTCOME YOU'VE EXPERIENCED].
            Drop a [EMOJI] if you've [EXPERIENCED SIMILAR THING] too.`;
            break;
          case 17:
            templateAndStrategy = `Day 17: Experience/Client Thread - Template: "Life Happens But"
            [UNEXPECTED LIFE EVENT/CHALLENGE] 🥲
            But still [DOING YOUR THING] because [YOUR CORE PRINCIPLE].
            Your [BUSINESS/GOALS] doesn't stop for [CHALLENGES] - your [SYSTEMS/HABITS] should [WORK WITHOUT YOU/BE RESILIENT].`;
            break;
          case 18:
            templateAndStrategy = `Day 18: Engagement Thread - Template: "If [EXTREME EXAMPLE] Can Do It"
            If [PEOPLE IN UNLIKELY SITUATION] can find the means to [ACCOMPLISH TASK], then you my friend can [SIMPLE TASK YOUR AUDIENCE SHOULD DO].
            What's your excuse today? (asking with love 💕)`;
            break;
          case 19:
            templateAndStrategy = `Day 19: Experience/Client Thread - Template: "I've Seen a Spike"
            I've seen a SPIKE in [POSITIVE METRIC] since [CHANGE YOU MADE].
            [PHENOMENON/STRATEGY] is real, people.
            One [ACTION] can ripple across your entire [BUSINESS/LIFE/RESULTS].`;
            break;
          case 20:
            templateAndStrategy = `Day 20: Funny/Relatable Thread - Template: "Generational Crisis"
            THIS MUST BE FUNNY ABOUT LIFE, NOT BUSINESS!
            [YOUNG PERSON BEHAVIOR/REFERENCE] and I just [REACTION THAT SHOWS YOUR AGE].
            Wait... you were [YOUNG PERSON MILESTONE] in [RECENT YEAR]?
            *[YOUR GENERATION] crisis intensifies* 👵/👴
            
            EXAMPLES OF WHAT THIS SHOULD LOOK LIKE:
            - "My kid called a USB a 'save button' and I felt ancient"
            - "Gen Z said my side part is cheugy and I don't even know what that means"
            - "Realized I'm now the age my parents were when I thought they were OLD"
            
            DO NOT MAKE THIS ABOUT BUSINESS OR SYSTEMS OR CLIENTS!`;
            break;
          case 21:
            templateAndStrategy = `Day 21: Power Statement Thread - Template: "Marked Safe From"
            Marked safe from [UNNECESSARY EXPENSE/COMMON MISTAKE] because I was busy working on [WHAT YOU DID FOR YOUR BUSINESS TODAY].`;
            break;
        }
      } else if (day >= 22 && day <= 30) {
        // Week 4: Thought Leadership templates
        switch (day) {
          case 22:
            templateAndStrategy = `Day 22: Strategic Promotional Thread - Template: "The Problem Isn't Your [X]"
            [HISTORICAL REFERENCE] charged us [AMOUNT] for [SERVICE] and we paid it without question.
            Now [TARGET MARKET] [NEGOTIATE/COMPLAIN] over a [YOUR PRICE/OFFERING].
            The problem isn't your [ASSUMED ISSUE]. The problem is [REAL ISSUE].
            [LINK TO SERVICES/BOOKING PAGE]`;
            break;
          case 23:
            templateAndStrategy = `Day 23: Industry Observation Thread - Template: "Hot Take"
            Hot take: [CONTROVERSIAL BUT TRUE STATEMENT ABOUT YOUR INDUSTRY].
            [SUPPORTING EVIDENCE OR REASONING - 1-2 sentences].
            Change my mind. (But you probably can't because [CREDIBILITY STATEMENT].)`;
            break;
          case 24:
            templateAndStrategy = `Day 24: Power Statement Thread - Template: "The Hardest Part About Being [X]"
            The hardest part about being [YOUR TYPE/ROLE] isn't [EXPECTED CHALLENGE].
            It's [UNEXPECTED EMOTIONAL/INTERNAL CHALLENGE].
            But we're too [TRAIT] to admit [VULNERABILITY].`;
            break;
          case 25:
            templateAndStrategy = `Day 25: Funny/Relatable Thread - Template: "Plot Twist"
            THIS MUST BE FUNNY ABOUT LIFE, NOT BUSINESS!
            Plot twist: [COMMON ASSUMPTION ABOUT YOUR BUSINESS/LIFE]
            Reality: [WHAT IT'S ACTUALLY LIKE - FUNNY OR SURPRISING]
            The gap between expectation and reality is [METAPHOR]. 😅
            
            EXAMPLES OF WHAT THIS SHOULD LOOK LIKE:
            - "Plot twist: Being self-employed means I can work whenever I want. Reality: I work ALL the time and feel guilty when I don't"
            - "Plot twist: Working from home is so relaxing. Reality: I haven't changed out of pajamas in 3 days and my couch is my office"
            
            DO NOT MAKE THIS ABOUT BUSINESS SYSTEMS OR PROFESSIONAL ADVICE!`;
            break;
          case 26:
            templateAndStrategy = `Day 26: Power Statement Thread - Template: "When You Get Both [X] and [Y] Right"
            When you get both [COMPONENT 1] AND [COMPONENT 2] right...
            That's when [BUSINESSES/PEOPLE/RESULTS] actually [SUCCEED] instead of just [SPINNING WHEELS/STRUGGLING].
            [EITHER/OR THINKING] won't get you there.`;
            break;
          case 27:
            templateAndStrategy = `Day 27: Industry Observation Thread - Template: "Everyone's Talking About [X], But"
            Everyone's talking about [TRENDING TOPIC/STRATEGY], but nobody's mentioning [THE MISSING PIECE].
            You can [DO THE TRENDY THING] all you want.
            Without [YOUR EXPERTISE/MISSING PIECE], you're just [METAPHOR FOR FUTILE EFFORT].`;
            break;
          case 28:
            templateAndStrategy = `Day 28: Funny/Relatable Thread - Template: "My [PERSONALITY TRAIT] Can't Handle"
            THIS MUST BE FUNNY ABOUT LIFE, NOT BUSINESS!
            My [PERSONALITY TRAIT/TYPE] can't handle [SCENARIO THAT CONFLICTS WITH YOUR NATURE].
            Like asking a [METAPHOR] to [OPPOSITE BEHAVIOR].
            It's not happening. And I've made peace with that. ✌️
            
            EXAMPLES OF WHAT THIS SHOULD LOOK LIKE:
            - "My introvert brain can't handle surprise phone calls. Like asking a cat to enjoy a bath. Not happening."
            - "My perfectionist soul can't handle 'good enough'. Like asking a chef to serve burnt toast. Nope."
            
            DO NOT MAKE THIS ABOUT BUSINESS SYSTEMS OR PROFESSIONAL ADVICE!`;
            break;
          case 29:
            templateAndStrategy = `Day 29: Industry Observation Thread - Template: "Stop [COMMON BEHAVIOR] and Start [BETTER BEHAVIOR]"
            Stop [COMMON INEFFECTIVE BEHAVIOR IN YOUR INDUSTRY].
            Start [YOUR RECOMMENDED APPROACH].
            The difference? [SPECIFIC OUTCOME/BENEFIT].
            Your [TARGET MARKET] will thank you.`;
            break;
          case 30:
            templateAndStrategy = `Day 30: Experience/Client Thread + Challenge Wrap - Template: "30 Days Later"
            30 days of [THE CHALLENGE] later and here's what I learned:
            [KEY INSIGHT 1] [KEY INSIGHT 2] [KEY INSIGHT 3]
            If you followed along, drop your biggest win below 👇
            Next up: [WHAT'S COMING NEXT FOR YOU]`;
            break;
        }
      }

      const prompt = `You are Batch Bestie, an expert at creating engaging Threads content for ${userProfile.industry} professionals. 

WEEK 1 STRATEGY: Foundation Building - Establish authority and challenge common misconceptions in the industry.

USER PROFILE:
- Industry: ${userProfile.industry}
- Ideal Client: ${userProfile.idealClient}
- Transformation: ${userProfile.transformation}
- Unique Edge: ${userProfile.uniqueEdge}
- Voice Style: ${userProfile.voiceStyle}
- Traffic Destination: ${userProfile.trafficDestination}
- Personality Themes: ${userProfile.personalityThemes.join(', ')}
- Best Content Examples: ${userProfile.bestContent}
- Client Testimonials: ${userProfile.clientTestimonials}
${userProfile.enneagramType ? `- Enneagram Type: ${userProfile.enneagramType}` : ''}

TEMPLATE TO USE:
${templateAndStrategy}

CRITICAL CHARACTER LIMITS:
- Keep the thread to 500 characters or less for a single post
- If you need more space, format it as multiple threads with clear breaks
- Mark thread breaks with "---THREAD BREAK---" where users should split
- MAXIMUM 1500 characters total (3 threads max)
- Prioritize brevity and punch over length

Create a ${type} thread for Day ${day} that:
1. Uses the template structure provided
2. Incorporates the user's specific industry, ideal client, and unique edge naturally
3. Matches their voice style (${userProfile.voiceStyle})
4. Includes relevant personality themes where appropriate
5. Drives traffic to: ${userProfile.trafficDestination}
6. Sounds authentic and engaging
7. Is ready to post on Threads (proper formatting, emojis where appropriate)
8. STAYS UNDER 500 characters if possible, or clearly marks thread breaks

Write the complete thread content. Do not include explanations or brackets - just the final thread ready to post.`;

      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          messages: [
            { role: "user", content: prompt }
          ]
        })
      });

      if (!response.ok) {
        throw new Error(`Claude API request failed: ${response.status}`);
      }

      const data = await response.json();
      return data.content[0].text;

    } catch (error) {
      console.error('Error calling Claude API:', error);
      // Fallback to original generation method
      return generateThreadFallback(type, day);
    }
  };

  const generateThreadFallback = (type, day) => {
    // Enhanced fallback method using the new content structures and user profile
    const industry = userProfile.industry || "business";
    const voiceStyle = userProfile.voiceStyle || "professional";
    const uniqueEdge = userProfile.uniqueEdge || "proven approach";
    const idealClient = userProfile.idealClient || "business owners";
    const trafficDestination = userProfile.trafficDestination || "link in bio";
    
    switch (type) {
      case "Strategic Promo":
        return `Just dropped something that shows ${idealClient.toLowerCase()} how to ${getMainBenefit()}.\n\nWhy this matters:\nMost people in ${industry} are still [common struggle].\n\nGrab it here: ${trafficDestination}`;
        
      case "Power Statement":
        // Use the Power Statement format from examples
        const misconception = getCommonBadAdvice();
        return `Most ${industry} experts tell you to ${misconception}.\n\nBut here's the reality:\n${uniqueEdge}\n\nSee how this changes everything?`;
        
      case "Client Experience":
        // Use Experience/Client format
        return `Started in ${industry} feeling [struggle].\n\nAfter [timeframe] of implementing ${uniqueEdge}:\n• [Client result 1]\n• [Client result 2]\n\nNow I help ${idealClient.toLowerCase()} do the same.`;
        
      case "Funny/Relatable":
        // Use Funny/Relatable format 
        const personalityTouch = getPersonalityTouch();
        return `No one will hype up your ${industry} strategy as much as [relatable comparison]. 😂\n\n${personalityTouch}\n\nWhy are we like this?`;
        
      case "Industry Observation":
        // Use Industry Observation/Callout format
        return `I'm going to be honest with you.\n\n[Common ${industry} frustration that everyone experiences].\n\nNormalize calling this out.`;
        
      default:
        return `${type} thread for day ${day} - personalized for ${industry} using ${voiceStyle} voice and featuring ${uniqueEdge}`;
    }
  };

  // Enhanced helper functions with error handling
  const getCommonBadAdvice = () => {
    try {
      const badAdvice = {
        "Digital marketing": "post more content and hope for the best",
        "Real estate": "cold call everyone in your area",
        "Coaching": "lower your prices to get more clients",
        "SaaS": "add more features to beat the competition"
      };
      return badAdvice[userProfile.industry] || "follow the same strategies everyone else uses";
    } catch (error) {
      console.error('Error in getCommonBadAdvice:', error);
      return "follow outdated advice";
    }
  };

  const getShortEdge = () => {
    try {
      const edge = userProfile.uniqueEdge || "focusing on systems over tactics";
      // Don't truncate - return the full edge statement
      return edge;
    } catch (error) {
      console.error('Error in getShortEdge:', error);
      return "my proven method";
    }
  };

  const getMainBenefit = () => {
    try {
      const transformation = userProfile.transformation || "scale your business without the overwhelm";
      // Return the full transformation, not truncated
      return transformation.toLowerCase().replace(/help|assist/g, '').trim();
    } catch (error) {
      console.error('Error in getMainBenefit:', error);
      return "achieve better results";
    }
  };

  const getPainPoint = () => {
    try {
      const clientDesc = userProfile.idealClient || "business owners";
      return `Most ${clientDesc.split(' ').slice(0, 3).join(' ')} are spinning their wheels on tactics that don't actually move the needle.`;
    } catch (error) {
      console.error('Error in getPainPoint:', error);
      return "Most people are struggling with the same challenges you've faced.";
    }
  };

  const getTrafficDestination = () => {
    try {
      const destination = userProfile.trafficDestination || "link in bio";
      // Return more complete destination info instead of truncating
      if (destination.includes('http')) {
        // Extract URL and keep it complete
        const urlMatch = destination.match(/(https?:\/\/[^\s]+)/);
        if (urlMatch) return urlMatch[1];
      }
      // If it's a description, return the first meaningful part
      const words = destination.split(' ');
      if (words.length <= 6) return destination;
      return words.slice(0, 6).join(' ') + '...';
    } catch (error) {
      console.error('Error in getTrafficDestination:', error);
      return "link in bio";
    }
  };

  const getClientName = () => {
    return "[Client Name - e.g., Sarah, Jessica, etc.]";
  };

  const getClientProblem = () => {
    return "[describe their problem - e.g., feeling overwhelmed and stuck at $5K months, struggling with lead generation, etc.]";
  };

  const getRevenueLevel = () => {
    return "[their starting revenue - e.g., $50K, $75K, 6-figures, etc.]";
  };

  const getSpecificResult = () => {
    return "[enter specific result - e.g., doubled her revenue, hit first $10K month, etc.]";
  };

  const getBonusResult = () => {
    return "[add secondary benefit - e.g., finally took that 2-week vacation, reduced work hours by 50%, etc.]";
  };

  const getImpressiveResult = () => {
    return "[enter impressive milestone - e.g., first $25K month, 6-figures in 90 days, 300% ROI, etc.]";
  };

  const getBeforeState = () => {
    return "[describe their before state - e.g., working 60-hour weeks for $3K months, stuck at $50K, etc.]";
  };

  const getPersonalityTouch = () => {
    try {
      const touches = {
        "Enneagram insights": getEnneagramTouch(),
        "Parenting/motherhood": getParentingTouch(),
        "Millennial experiences": "(Peak millennial behavior right here)",
        "Pop culture references": getPopCultureTouch(),
        "Entrepreneurship journey": getEntrepreneurTouch(),
        "Food/cooking": getCookingTouch(),
        "Fitness/wellness": getFitnessTouch(),
        "Faith/spirituality": getFaithTouch(),
        "Travel stories": getTravelTouch(),
        "Sports analogies": getSportsTouch(),
        "Music references": getMusicTouch()
      };
      const theme = userProfile.personalityThemes[0];
      return touches[theme] || "(Why are we like this?)";
    } catch (error) {
      console.error('Error in getPersonalityTouch:', error);
      return "";
    }
  };

  const getEnneagramTouch = () => {
    const type = userProfile.enneagramType;
    if (type.includes('Type 3')) return "(Classic Type 3 energy, anyone?)";
    if (type.includes('Type 1')) return "(My Type 1 brain is screaming)";
    if (type.includes('Type 2')) return "(Type 2 helping everyone but myself)";
    if (type.includes('Type 4')) return "(Type 4 feelings activated)";
    if (type.includes('Type 7')) return "(Type 7 chaos incoming)";
    if (type.includes('Type 8')) return "(Type 8 has entered the chat)";
    return "(Enneagram besties understand)";
  };

  const getParentingTouch = () => {
    const style = userProfile.parentingStyle.toLowerCase();
    const ages = userProfile.kidAges.toLowerCase();
    
    if (style.includes('wine mom')) return "(Wine mom energy activated)";
    if (style.includes('boy mom')) return "(Boy mom chaos level: maximum)";
    if (style.includes('working mom')) return "(Working mom juggling act in progress)";
    if (style.includes('crunchy')) return "(Crunchy mama vibes)";
    if (ages.includes('toddler')) return "(Toddler chaos in the background)";
    if (ages.includes('teen')) return "(Teen attitude levels rising)";
    return "(Mom life is wild)";
  };

  const getPopCultureTouch = () => {
    const vibe = userProfile.popCultureVibe.toLowerCase();
    if (vibe.includes('taylor swift')) return "(Swiftie energy activated)";
    if (vibe.includes('reality tv')) return "(Reality TV addict checking in)";
    if (vibe.includes('marvel')) return "(Marvel nerd alert)";
    if (vibe.includes('true crime')) return "(True crime junkie vibes)";
    return "(Main character energy)";
  };

  const getEntrepreneurTouch = () => {
    const story = userProfile.entrepreneurStory.toLowerCase();
    if (story.includes('corporate dropout')) return "(Corporate dropout survivor)";
    if (story.includes('garage')) return "(Started in the garage energy)";
    if (story.includes('failed')) return "(Failed my way to success)";
    return "(Entrepreneur life chose me)";
  };

  const getCookingTouch = () => {
    const style = userProfile.cookingStyle.toLowerCase();
    if (style.includes('30-minute')) return "(30-minute meals or we starve)";
    if (style.includes('sourdough')) return "(Sourdough obsession is real)";
    if (style.includes('meal prep')) return "(Meal prep queen energy)";
    if (style.includes('takeout')) return "(Takeout survivor checking in)";
    return "(Kitchen chaos level: moderate)";
  };

  const getFitnessTouch = () => {
    const style = userProfile.fitnessStyle.toLowerCase();
    if (style.includes('5am')) return "(5am gym mom energy)";
    if (style.includes('yoga')) return "(Namaste vibes)";
    if (style.includes('walking meetings')) return "(Walking meeting convert)";
    if (style.includes('couch potato')) return "(Reformed couch potato)";
    return "(Wellness journey in progress)";
  };

  const getFaithTouch = () => {
    const background = userProfile.faithBackground.toLowerCase();
    if (background.includes('christian')) return "(Faith + business combo)";
    if (background.includes('manifesting')) return "(Manifesting mindset activated)";
    if (background.includes('meditation')) return "(Meditation practice paying off)";
    return "(Faith over fear)";
  };

  const getTravelTouch = () => {
    const style = userProfile.travelStyle.toLowerCase();
    if (style.includes('nomad')) return "(Digital nomad life)";
    if (style.includes('road trips')) return "(Road trip warrior)";
    if (style.includes('luxury')) return "(Luxury retreat vibes)";
    if (style.includes('budget')) return "(Budget travel veteran)";
    return "(Wanderlust activated)";
  };

  const getSportsTouch = () => {
    const background = userProfile.sportsBackground.toLowerCase();
    if (background.includes('college athlete')) return "(Former college athlete energy)";
    if (background.includes('fantasy football')) return "(Fantasy football obsessed)";
    if (background.includes('soccer mom')) return "(Soccer mom sideline vibes)";
    if (background.includes('marathon')) return "(Marathon mentality)";
    return "(Competitive spirit activated)";
  };

  const getMusicTouch = () => {
    const taste = userProfile.musicTaste.toLowerCase();
    if (taste.includes('taylor swift')) return "(Swiftie energy activated)";
    if (taste.includes('indie')) return "(Indie vibes)";
    if (taste.includes('country')) return "(Country music soul)";
    if (taste.includes('rock')) return "(Rock and roll spirit)";
    return "(Music lover checking in)";
  };

  const getTestimonialContent = () => {
    const testimonials = userProfile.clientTestimonials || "";
    if (testimonials) {
      // Parse the testimonials and randomly select one
      const testimonialLines = testimonials.split('\n').filter(line => line.trim());
      if (testimonialLines.length > 0) {
        const randomTestimonial = testimonialLines[Math.floor(Math.random() * testimonialLines.length)];
        return `"${randomTestimonial.trim()}"`;
      }
    }
    // Provide bracket placeholder instead of made-up testimonial
    return `"[Insert client testimonial here - include client name/initial, their result, and exact quote. Example: Sarah K. hit $15K months in 90 days: 'I can't believe how fast this worked!']"`;
  };

  const getTestimonialCTA = () => {
    const ctas = [
      "Ready for your own success story?",
      "Want results like this?", 
      "This could be you next.",
      "Ready to join them?",
      "Your turn. Let's make it happen."
    ];
    return ctas[Math.floor(Math.random() * ctas.length)];
  };

  const getTrendingTopic = () => {
    const topics = {
      "Digital marketing": "AI tools and automation",
      "Real estate": "social media marketing", 
      "Coaching": "personal branding",
      "SaaS": "product-led growth"
    };
    return topics[userProfile.industry] || "the latest shiny object";
  };

  const getKeyInsight = () => {
    const insights = {
      "bold": "simple systems and consistent execution",
      "snarky": "doing the boring stuff that actually works",
      "relatable": "mastering the fundamentals", 
      "professional": "proven frameworks and data-driven decisions"
    };
    return insights[userProfile.voiceStyle] || "the basics done really well";
  };

  const getControversialTake = () => {
    const takes = {
      "Digital marketing": "Most marketing courses are teaching outdated strategies from 2019.",
      "Real estate": "Cold calling is dead. Stop wasting everyone's time.",
      "Coaching": "90% of business coaches have never actually built a successful business.",
      "SaaS": "Your customers don't want more features. They want fewer clicks."
    };
    return takes[userProfile.industry] || `Most ${userProfile.industry} advice is completely backwards.`;
  };

  const getBadAdviceExample = () => {
    return `"Just post 10 times a day and you'll blow up!" (Spoiler: I didn't blow up. I burned out.)`;
  };

  // Simple export function with tooltip
  const handleExportAll = () => {
    console.log('Export All button clicked');
    console.log('Current batchContent:', batchContent);
    
    setShowExportTooltip(!showExportTooltip);
  };

  // Export CSV function - completely rewritten 
  const doExport = () => {
    console.log('doExport called'); // Debug log
    console.log('batchContent for export:', batchContent); // Debug log
    
    try {
      const csvRows = [];
      
      // Add header
      csvRows.push(['Week', 'Day', 'Type', 'Content']);
      
      // Process each week 1-4
      for (let weekNum = 1; weekNum <= 4; weekNum++) {
        const weekData = weeklyFramework[weekNum];
        if (!weekData) continue;
        
        let weekHasContent = false;
        const weekRows = [];
        
        // Check each day in this week
        Object.entries(weekData.days).forEach(([dayStr, dayInfo]) => {
          const dayNum = parseInt(dayStr);
          const existingThread = batchContent.find(item => item.day === dayNum);
          
          if (existingThread && existingThread.content && !existingThread.isGenerating && !existingThread.hasError) {
            weekHasContent = true;
            weekRows.push([
              `Week ${weekNum}`,
              `Day ${dayStr}`,
              dayInfo.type,
              existingThread.content
            ]);
          }
        });
        
        if (weekHasContent) {
          // Add all the actual content for this week
          csvRows.push(...weekRows);
        } else {
          // Add placeholder row for this week
          csvRows.push([
            `Week ${weekNum}`,
            'Not Generated',
            'All Types',
            `Week ${weekNum} not generated - use the dashboard to generate this week's content`
          ]);
        }
      }
      
      console.log('CSV rows to export:', csvRows); // Debug log
      
      // Convert to CSV string
      const csvContent = csvRows.map(row => 
        row.map(field => {
          const cleanField = String(field || '').replace(/"/g, '""');
          return `"${cleanField}"`;
        }).join(',')
      ).join('\r\n');
      
      console.log('Generated CSV content length:', csvContent.length); // Debug log
      
      // Create and download file
      const blob = new Blob(['\uFEFF' + csvContent], { 
        type: 'text/csv;charset=utf-8;' 
      });
      
      const link = document.createElement('a');
      const url = URL.createObjectURL(blob);
      link.setAttribute('href', url);
      link.setAttribute('download', 'batch-bestie-all-content.csv');
      link.style.visibility = 'hidden';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      URL.revokeObjectURL(url);
      
      // Close modal and show success message
      setShowExportModal(false);
      setUploadError('✅ Export completed successfully!');
      setTimeout(() => setUploadError(''), 3000);
      
    } catch (error) {
      console.error('Error in doExport:', error);
      setUploadError('Export failed. Please check the console for details.');
      setTimeout(() => setUploadError(''), 5000);
    }
  };

  const getClientQuote = () => {
    const quotes = [
      "I can't believe this actually works. I'm booked out for the next 3 months!",
      "I just hit my revenue goal... in half the time I expected.",
      "For the first time in years, I'm not working weekends.",
      "I finally feel like I know what I'm doing with my business."
    ];
    return quotes[Math.floor(Math.random() * quotes.length)];
  };

  const getEmotionalResponse = () => {
    return "My heart is so full. This is exactly why I do what I do.";
  };

  const generateBatchContent = async (week) => {
    try {
      console.log('Generating content for week:', week);
      setSelectedWeek(week);
      setCurrentView('batch'); // Go directly to batch view
      
      const weekData = weeklyFramework[week];
      console.log('Week data:', weekData);
      
      if (!weekData) {
        console.error('No week data found for week:', week);
        setUploadError(`Error: Week ${week} data not found. Please refresh and try again.`);
        setTimeout(() => setUploadError(''), 3000);
        setCurrentView('dashboard');
        return;
      }

      // Clear existing content for this week and set up placeholders
      const existingOtherWeeks = batchContent.filter(item => {
        const dayNumbers = Object.keys(weekData.days).map(d => parseInt(d));
        return !dayNumbers.includes(item.day);
      });
      
      // Create placeholders for all days in this week
      const placeholders = Object.entries(weekData.days).map(([day, info]) => ({
        day: parseInt(day),
        type: info.type,
        content: null, // null means still generating
        icon: info.icon,
        color: info.color,
        isGenerating: true
      }));
      
      setBatchContent([...existingOtherWeeks, ...placeholders].sort((a, b) => a.day - b.day));

      // Process each day in the week
      for (const [day, info] of Object.entries(weekData.days)) {
        const dayNum = parseInt(day);
        
        console.log('Processing day:', day, 'with info:', info);
        try {
          const content = await generateThread(info.type, dayNum);
          
          const completedThread = {
            day: dayNum,
            type: info.type,
            content: content,
            icon: info.icon,
            color: info.color,
            isGenerating: false
          };
          
          // Replace placeholder with completed content
          setBatchContent(prev => 
            prev.map(item => 
              item.day === dayNum ? completedThread : item
            )
          );
          
        } catch (threadError) {
          console.error('Error generating thread for day', day, ':', threadError);
          
          // Replace with error content
          const errorThread = {
            day: dayNum,
            type: info.type,
            content: `[Error generating ${info.type} content - please try regenerating]`,
            icon: info.icon,
            color: info.color,
            isGenerating: false,
            hasError: true
          };
          
          setBatchContent(prev => 
            prev.map(item => 
              item.day === dayNum ? errorThread : item
            )
          );
        }
      }
      
    } catch (error) {
      console.error('Critical error in generateBatchContent:', error);
      setUploadError('Something went wrong generating content. Please refresh the page and try again.');
      setTimeout(() => setUploadError(''), 3000);
      setCurrentView('dashboard');
    }
  };

  const handleInputChange = (field, value) => {
    // Simple input handling - no automatic corrections for normal typing
    let correctedValue = value;
    
    // Only apply special handling for URL fields
    if (field === 'trafficDestination' && typeof correctedValue === 'string') {
      correctedValue = correctedValue.replace(/(https?:\/\/[^\s]+)\s+/g, '$1');
      correctedValue = correctedValue.replace(/\s+(https?:\/\/)/g, ' $1');
      correctedValue = correctedValue.replace(/(www\.[^\s]+)\s+/g, '$1');
      correctedValue = correctedValue.replace(/\s+(www\.)/g, ' $1');
      correctedValue = correctedValue.replace(/(\w+)\s+\.\s+(\w+)/g, '$1.$2');
      correctedValue = correctedValue.replace(/(\w+)\s+\/\s+/g, '$1/');
      correctedValue = correctedValue.replace(/\/\s+(\w+)/g, '/$1');
    }
    
    setUserProfile(prev => ({ ...prev, [field]: correctedValue }));
  };

  const handleThemeToggle = (theme) => {
    setUserProfile(prev => ({
      ...prev,
      personalityThemes: prev.personalityThemes.includes(theme)
        ? prev.personalityThemes.filter(t => t !== theme)
        : [...prev.personalityThemes, theme]
    }));
  };

  // Download profile as CSV
  const downloadProfile = () => {
    try {
      const csvData = [
        ['Field', 'Value'],
        ['Industry', userProfile.industry || ''],
        ['Ideal Client', userProfile.idealClient || ''],
        ['Transformation', userProfile.transformation || ''],
        ['Unique Edge', userProfile.uniqueEdge || ''],
        ['Best Content', userProfile.bestContent || ''],
        ['Voice Style', userProfile.voiceStyle || ''],
        ['Client Testimonials', userProfile.clientTestimonials || ''],
        ['Client Success Stories', userProfile.clientStories || ''],
        ['Traffic Destination', userProfile.trafficDestination || ''],
        ['Personality Themes', userProfile.personalityThemes.join('; ') || ''],
        ['Enneagram Type', userProfile.enneagramType || ''],
        ['Parenting Style', userProfile.parentingStyle || ''],
        ['Kid Ages', userProfile.kidAges || ''],
        ['Cooking Style', userProfile.cookingStyle || ''],
        ['Fitness Style', userProfile.fitnessStyle || ''],
        ['Faith Background', userProfile.faithBackground || ''],
        ['Travel Style', userProfile.travelStyle || ''],
        ['Entrepreneur Story', userProfile.entrepreneurStory || ''],
        ['Pop Culture Vibe', userProfile.popCultureVibe || ''],
        ['Sports Background', userProfile.sportsBackground || ''],
        ['Music Taste', userProfile.musicTaste || '']
      ];

      // Create CSV content with proper escaping
      const csvContent = csvData.map(row => 
        row.map(field => {
          const cleanField = String(field).replace(/"/g, '""');
          return `"${cleanField}"`;
        }).join(',')
      ).join('\r\n');

      // Use Blob with explicit type
      const blob = new Blob(['\uFEFF' + csvContent], { 
        type: 'text/csv;charset=utf-8;' 
      });
      
      // Create download link
      const link = document.createElement('a');
      if (link.download !== undefined) {
        const url = URL.createObjectURL(blob);
        link.setAttribute('href', url);
        link.setAttribute('download', 'batch-bestie-profile.csv');
        link.style.visibility = 'hidden';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
      }
    } catch (error) {
      console.error('Download error:', error);
      // Fallback: show CSV content in a new window
      alert('Download failed. Your profile data will open in a new window - you can copy and save it manually.');
      const csvData = [
        ['Field', 'Value'],
        ['Industry', userProfile.industry || ''],
        ['Ideal Client', userProfile.idealClient || ''],
        ['Transformation', userProfile.transformation || ''],
        ['Unique Edge', userProfile.uniqueEdge || ''],
        ['Best Content', userProfile.bestContent || ''],
        ['Voice Style', userProfile.voiceStyle || ''],
        ['Client Testimonials', userProfile.clientTestimonials || ''],
        ['Traffic Destination', userProfile.trafficDestination || ''],
        ['Personality Themes', userProfile.personalityThemes.join('; ') || ''],
        ['Enneagram Type', userProfile.enneagramType || ''],
        ['Parenting Style', userProfile.parentingStyle || ''],
        ['Kid Ages', userProfile.kidAges || ''],
        ['Cooking Style', userProfile.cookingStyle || ''],
        ['Fitness Style', userProfile.fitnessStyle || ''],
        ['Faith Background', userProfile.faithBackground || ''],
        ['Travel Style', userProfile.travelStyle || ''],
        ['Entrepreneur Story', userProfile.entrepreneurStory || ''],
        ['Pop Culture Vibe', userProfile.popCultureVibe || ''],
        ['Sports Background', userProfile.sportsBackground || ''],
        ['Music Taste', userProfile.musicTaste || '']
      ];
      
      const csvContent = csvData.map(row => 
        row.map(field => `"${String(field).replace(/"/g, '""')}"`).join(',')
      ).join('\n');
      
      const newWindow = window.open();
      newWindow.document.write('<pre>' + csvContent + '</pre>');
    }
  };

  // Upload and parse profile CSV
  const handleProfileUpload = (event) => {
    const file = event.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = (e) => {
      try {
        const csv = e.target.result;
        const lines = csv.split('\n');
        const newProfile = { ...userProfile };

        console.log('Uploading CSV with', lines.length, 'lines');

        // Skip header row, process data rows
        for (let i = 1; i < lines.length; i++) {
          const line = lines[i].trim();
          if (!line) continue;

          // Parse CSV line (handle quotes and commas properly)
          const matches = [];
          let current = '';
          let inQuotes = false;
          let j = 0;
          
          while (j < line.length) {
            const char = line[j];
            const nextChar = line[j + 1];
            
            if (char === '"' && !inQuotes) {
              inQuotes = true;
            } else if (char === '"' && inQuotes && nextChar === '"') {
              current += '"';
              j++; // skip next quote
            } else if (char === '"' && inQuotes) {
              inQuotes = false;
            } else if (char === ',' && !inQuotes) {
              matches.push(current);
              current = '';
            } else {
              current += char;
            }
            j++;
          }
          matches.push(current); // add final field

          if (matches.length < 2) continue;

          const field = matches[0].trim();
          const value = matches[1].trim();

          // Map CSV fields to profile fields
          switch (field) {
            case 'Industry': newProfile.industry = value; break;
            case 'Ideal Client': newProfile.idealClient = value; break;
            case 'Transformation': newProfile.transformation = value; break;
            case 'Unique Edge': newProfile.uniqueEdge = value; break;
            case 'Best Content': newProfile.bestContent = value; break;
            case 'Voice Style': newProfile.voiceStyle = value; break;
            case 'Client Testimonials': newProfile.clientTestimonials = value; break;
            case 'Traffic Destination': newProfile.trafficDestination = value; break;
            case 'Personality Themes': 
              newProfile.personalityThemes = value ? value.split('; ').filter(t => t.trim()) : [];
              break;
            case 'Enneagram Type': newProfile.enneagramType = value; break;
            case 'Parenting Style': newProfile.parentingStyle = value; break;
            case 'Kid Ages': newProfile.kidAges = value; break;
            case 'Cooking Style': newProfile.cookingStyle = value; break;
            case 'Fitness Style': newProfile.fitnessStyle = value; break;
            case 'Faith Background': newProfile.faithBackground = value; break;
            case 'Travel Style': newProfile.travelStyle = value; break;
            case 'Entrepreneur Story': newProfile.entrepreneurStory = value; break;
            case 'Pop Culture Vibe': newProfile.popCultureVibe = value; break;
            case 'Sports Background': newProfile.sportsBackground = value; break;
            case 'Music Taste': newProfile.musicTaste = value; break;
          }
        }

        console.log('Loaded profile:', newProfile);
        setUserProfile(newProfile);
        setCurrentView('dashboard');
        setUploadError('Profile loaded successfully!');
        
        // Clear success message after 3 seconds
        setTimeout(() => setUploadError(''), 3000);
      } catch (error) {
        setUploadError('Error reading profile file. Please make sure it\'s a valid CSV.');
        console.error('Profile upload error:', error);
      }
    };

    reader.readAsText(file);
    event.target.value = ''; // Reset file input
  };

  const nextStep = () => {
    if (currentStep === questions.length - 1) {
      setCurrentView('profileComplete');
    } else {
      setCurrentStep(prev => prev + 1);
    }
  };

  const prevStep = () => {
    setCurrentStep(prev => Math.max(prev - 1, 0));
  };

  // Regenerate individual thread
  const regenerateThread = async (day) => {
    try {
      // Find the thread info
      const weekData = weeklyFramework[selectedWeek];
      const dayInfo = weekData.days[day];
      
      if (!dayInfo) {
        console.error('Day info not found for day:', day);
        return;
      }

      // Update thread to show it's regenerating
      setBatchContent(prev => 
        prev.map(item => 
          item.day === day 
            ? { ...item, isGenerating: true, content: null }
            : item
        )
      );

      const content = await generateThread(dayInfo.type, day);
      
      // Update with new content
      setBatchContent(prev => 
        prev.map(item => 
          item.day === day 
            ? { ...item, content: content, isGenerating: false, hasError: false }
            : item
        )
      );

    } catch (error) {
      console.error('Error regenerating thread for day', day, ':', error);
      
      // Show error state
      setBatchContent(prev => 
        prev.map(item => 
          item.day === day 
            ? { ...item, content: `[Error regenerating ${dayInfo.type} content]`, isGenerating: false, hasError: true }
            : item
        )
      );
    }
  };

  // Onboarding View
  if (currentView === 'onboarding') {
    const currentQuestion = questions[currentStep];
    const progress = ((currentStep + 1) / questions.length) * 100;

    return (
      <div className="max-w-2xl mx-auto p-6 bg-white min-h-screen">
        <div className="text-center mb-8">
          <div className="w-16 h-16 bg-gradient-to-r from-amber-800 to-amber-600 rounded-full flex items-center justify-center mx-auto mb-4">
            <Heart className="w-8 h-8 text-white" />
          </div>
          <h1 className="text-3xl font-bold text-gray-900 mb-2">Welcome to Batch Bestie</h1>
          <p className="text-gray-600">Let's create your personalized thread content strategy</p>
          
          {/* Upload Profile Option */}
          <div className="mt-6 p-4 bg-amber-50 border border-amber-200 rounded-lg">
            <h3 className="font-medium text-amber-900 mb-2">Already have a profile?</h3>
            <label className="inline-flex items-center px-4 py-2 bg-amber-600 text-white rounded-lg cursor-pointer hover:bg-amber-700 transition-colors">
              <Upload className="w-4 h-4 mr-2" />
              Upload Profile CSV
              <input
                type="file"
                accept=".csv"
                onChange={handleProfileUpload}
                className="hidden"
              />
            </label>
            {uploadError && (
              <p className="text-red-600 text-sm mt-2">{uploadError}</p>
            )}
          </div>
        </div>

        <div className="mb-8">
          <div className="flex justify-between text-sm text-gray-500 mb-2">
            <span>Step {currentStep + 1} of {questions.length}</span>
            <span>{Math.round(progress)}% Complete</span>
          </div>
          <div className="w-full bg-gray-200 rounded-full h-2">
            <div 
              className="bg-amber-600 h-2 rounded-full transition-all duration-300"
              style={{ width: `${progress}%` }}
            />
          </div>
        </div>

        <div className="mb-8">
          <div className="flex items-center mb-4">
            <div className="w-12 h-12 bg-amber-100 rounded-full flex items-center justify-center mr-4">
              <currentQuestion.icon className="w-6 h-6 text-amber-600" />
            </div>
            <div>
              <h1 className="text-2xl font-bold text-gray-900">{currentQuestion.title}</h1>
              <p className="text-gray-600">{currentQuestion.subtitle}</p>
            </div>
          </div>

          <div className="space-y-4">
            {currentQuestion.type === 'text' && (
              <input
                type="text"
                value={userProfile[currentQuestion.field]}
                onChange={(e) => handleInputChange(currentQuestion.field, e.target.value)}
                placeholder={currentQuestion.placeholder}
                className="w-full p-4 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent"
              />
            )}

            {currentQuestion.type === 'textarea' && (
              <textarea
                value={userProfile[currentQuestion.field]}
                onChange={(e) => handleInputChange(currentQuestion.field, e.target.value)}
                placeholder={currentQuestion.placeholder}
                rows={4}
                className="w-full p-4 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent"
              />
            )}

            {currentQuestion.type === 'radio' && (
              <div className="grid md:grid-cols-2 gap-4">
                {voiceOptions.map(option => (
                  <div
                    key={option.id}
                    onClick={() => handleInputChange('voiceStyle', option.id)}
                    className={`p-4 border-2 rounded-lg cursor-pointer transition-all ${
                      userProfile.voiceStyle === option.id
                        ? 'border-amber-500 bg-amber-50'
                        : 'border-gray-200 hover:border-gray-300'
                    }`}
                  >
                    <div className="flex items-center mb-2">
                      <option.icon className="w-5 h-5 text-amber-600 mr-2" />
                      <h3 className="font-semibold">{option.label}</h3>
                    </div>
                    <p className="text-sm text-gray-600">{option.desc}</p>
                  </div>
                ))}
              </div>
            )}

            {currentQuestion.type === 'checkbox' && (
              <div className="space-y-4">
                <div className="grid md:grid-cols-2 gap-2">
                  {personalityThemes.map(theme => (
                    <label
                      key={theme}
                      className="flex items-center p-3 border rounded-lg cursor-pointer hover:bg-gray-50"
                    >
                      <input
                        type="checkbox"
                        checked={userProfile.personalityThemes.includes(theme)}
                        onChange={() => handleThemeToggle(theme)}
                        className="mr-3 text-amber-600"
                      />
                      <span className="text-sm">{theme}</span>
                    </label>
                  ))}
                </div>
                
                {/* Conditional input for Enneagram type */}
                {userProfile.personalityThemes.includes('Enneagram insights') && (
                  <div className="mt-4 p-4 bg-amber-50 border border-amber-200 rounded-lg">
                    <label className="block text-sm font-medium text-amber-900 mb-2">
                      What's your Enneagram type?
                    </label>
                    <input
                      type="text"
                      value={userProfile.enneagramType}
                      onChange={(e) => handleInputChange('enneagramType', e.target.value)}
                      placeholder="e.g., Type 3, 6w5, Type 8w7, etc."
                      className="w-full p-3 border border-amber-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent bg-white"
                    />
                    <p className="text-xs text-amber-700 mt-1">
                      Enter your main type, with wing if you know it (like "3w4" or "Type 7w8")
                    </p>
                  </div>
                )}
              </div>
            )}
            {currentQuestion.type === 'conditional' && (
              <div className="space-y-4">
                {getConditionalQuestions(userProfile.personalityThemes).map((question, index) => (
                  <div key={index} className="border border-gray-200 rounded-lg p-4">
                    <label className="block text-sm font-medium text-gray-700 mb-2">
                      {question.label}
                    </label>
                    {question.type === 'select' ? (
                      <select
                        value={userProfile[question.field]}
                        onChange={(e) => handleInputChange(question.field, e.target.value)}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent"
                      >
                        <option value="">Select an option...</option>
                        {question.options.map(option => (
                          <option key={option} value={option}>{option}</option>
                        ))}
                      </select>
                    ) : (
                      <input
                        type="text"
                        value={userProfile[question.field]}
                        onChange={(e) => handleInputChange(question.field, e.target.value)}
                        placeholder={question.placeholder}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent"
                      />
                    )}
                  </div>
                ))}
                {getConditionalQuestions(userProfile.personalityThemes).length === 0 && (
                  <div className="text-center py-8 text-gray-500">
                    <Heart className="w-12 h-12 mx-auto mb-4 text-gray-300" />
                    <p>Select your personality themes in the previous step to unlock personalized questions!</p>
                  </div>
                )}
              </div>
            )}
          </div>
        </div>

        <div className="flex justify-between">
          <button
            onClick={prevStep}
            disabled={currentStep === 0}
            className="flex items-center px-6 py-3 text-gray-600 disabled:opacity-50 disabled:cursor-not-allowed hover:text-gray-900 transition-colors"
          >
            <ArrowLeft className="w-4 h-4 mr-2" />
            Previous
          </button>

          <button
            onClick={nextStep}
            disabled={
              (currentQuestion.type === 'conditional' && getConditionalQuestions(userProfile.personalityThemes).some(q => !userProfile[q.field])) ||
              (currentQuestion.type !== 'conditional' && currentQuestion.type !== 'checkbox' && !userProfile[currentQuestion.field]) || 
              (currentQuestion.type === 'checkbox' && userProfile.personalityThemes.length === 0)
            }
            className="flex items-center px-6 py-3 bg-gradient-to-r from-amber-800 to-amber-600 text-white rounded-lg font-semibold disabled:opacity-50 disabled:cursor-not-allowed hover:from-amber-900 hover:to-amber-700 transition-all duration-200"
          >
            {currentStep === questions.length - 1 ? 'Start Creating' : 'Next'}
            <ArrowRight className="w-4 h-4 ml-2" />
          </button>
        </div>
      </div>
    );
  }

  // Profile Complete View
  if (currentView === 'profileComplete') {
    return (
      <div className="max-w-2xl mx-auto p-6 bg-white min-h-screen">
        <div className="text-center mb-8">
          <div className="w-16 h-16 bg-gradient-to-r from-green-600 to-green-500 rounded-full flex items-center justify-center mx-auto mb-4">
            <Check className="w-8 h-8 text-white" />
          </div>
          <h1 className="text-3xl font-bold text-gray-900 mb-2">Profile Complete! 🎉</h1>
          <p className="text-gray-600">Your Batch Bestie is ready to create amazing content for you</p>
        </div>

        <div className="bg-amber-50 border border-amber-200 rounded-lg p-6 mb-6">
          <h3 className="font-semibold text-amber-900 mb-4">Save Your Profile</h3>
          <p className="text-amber-800 text-sm mb-4">
            Download your profile so you don't have to fill it out again next time!
          </p>
          <button
            onClick={downloadProfile}
            className="flex items-center px-4 py-2 bg-amber-600 text-white rounded-lg hover:bg-amber-700 transition-colors"
          >
            <Download className="w-4 h-4 mr-2" />
            Download My Profile
          </button>
        </div>

        <div className="space-y-4">
          <h3 className="font-semibold text-gray-900">Your Profile Summary:</h3>
          <div className="bg-gray-50 rounded-lg p-4 space-y-2 text-sm">
            <div><span className="font-medium">Industry:</span> {userProfile.industry}</div>
            <div><span className="font-medium">Voice:</span> {userProfile.voiceStyle}</div>
            <div><span className="font-medium">Personality Themes:</span> {userProfile.personalityThemes.join(', ')}</div>
          </div>
        </div>

        <div className="flex justify-center mt-8">
          <button
            onClick={() => setCurrentView('dashboard')}
            className="flex items-center px-8 py-3 bg-gradient-to-r from-amber-800 to-amber-600 text-white rounded-lg font-semibold hover:from-amber-900 hover:to-amber-700 transition-all duration-200"
          >
            Start Creating Content
            <ArrowRight className="w-4 h-4 ml-2" />
          </button>
        </div>
      </div>
    );
  }

  // Calendar View
  if (currentView === 'calendar') {
    // Create calendar data structure
    const calendarDays = [];
    for (let day = 1; day <= 30; day++) {
      const weekData = weeklyFramework[Math.ceil(day / 7) <= 4 ? Math.ceil(day / 7) : 4];
      const dayInfo = weekData?.days[day];
      const existingThread = batchContent.find(item => item.day === day);
      
      calendarDays.push({
        day,
        week: day <= 7 ? 1 : day <= 14 ? 2 : day <= 21 ? 3 : 4,
        type: dayInfo?.type || 'Content',
        icon: dayInfo?.icon || MessageCircle,
        color: dayInfo?.color || 'bg-gray-400',
        hasContent: existingThread && existingThread.content && !existingThread.isGenerating && !existingThread.hasError,
        content: existingThread?.content || null
      });
    }

    // Get current date for highlighting today
    const today = new Date();
    const currentDay = today.getDate();
    const currentMonth = today.getMonth();
    const currentYear = today.getFullYear();

    return (
      <div className="max-w-7xl mx-auto p-6 bg-white min-h-screen">
        <div className="flex items-center justify-between mb-8">
          <div className="flex items-center">
            <div className="w-12 h-12 bg-gradient-to-r from-amber-800 to-amber-600 rounded-full flex items-center justify-center mr-4">
              <Calendar className="w-6 h-6 text-white" />
            </div>
            <div>
              <h1 className="text-2xl font-bold text-gray-900">30-Day Threads Calendar</h1>
              <p className="text-gray-600">Your complete content strategy overview</p>
            </div>
          </div>
          <button
            onClick={() => setCurrentView('dashboard')}
            className="text-amber-700 hover:text-amber-800 font-medium"
          >
            ← Back to Dashboard
          </button>
        </div>

        {/* Week Legend */}
        <div className="grid md:grid-cols-4 gap-4 mb-8">
          {Object.entries(weeklyFramework).map(([weekNum, week]) => (
            <div key={weekNum} className="bg-gray-50 rounded-lg p-4 border">
              <h3 className="font-semibold text-gray-900 mb-1">Week {weekNum}</h3>
              <h4 className="text-sm font-medium text-amber-600 mb-1">{week.title}</h4>
              <p className="text-xs text-gray-500">{week.subtitle}</p>
            </div>
          ))}
        </div>

        {/* Calendar Grid */}
        <div className="bg-white rounded-lg shadow-sm border border-gray-200 overflow-hidden">
          <div className="grid grid-cols-7 gap-0">
            {/* Calendar Headers */}
            {['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].map(day => (
              <div key={day} className="bg-gray-100 px-4 py-3 text-center text-sm font-medium text-gray-700 border-r border-gray-200 last:border-r-0">
                {day}
              </div>
            ))}

            {/* Calendar Days */}
            {Array.from({ length: 35 }, (_, index) => {
              const dayNumber = index + 1;
              const calendarDay = calendarDays.find(d => d.day === dayNumber);
              
              if (dayNumber > 30) {
                // Empty cells after day 30
                return (
                  <div key={index} className="h-36 bg-gray-50 border-r border-b border-gray-200 last:border-r-0"></div>
                );
              }

              const IconComponent = calendarDay?.icon || MessageCircle;
              
              return (
                <div 
                  key={dayNumber} 
                  onClick={() => {
                    if (calendarDay?.hasContent) {
                      // Navigate to the batch view for this week
                      const weekNum = calendarDay.week;
                      setSelectedWeek(weekNum);
                      setCurrentView('batch');
                    }
                  }}
                  className={`h-36 border-r border-b border-gray-200 last:border-r-0 p-2 transition-colors ${
                    calendarDay?.hasContent 
                      ? 'bg-green-50 hover:bg-green-100 cursor-pointer' 
                      : 'bg-white hover:bg-gray-50'
                  }`}
                >
                  <div className="flex items-center justify-between mb-2">
                    <span className={`text-sm font-medium ${
                      calendarDay?.hasContent ? 'text-green-700' : 'text-gray-500'
                    }`}>
                      Day {dayNumber}
                    </span>
                    <div className={`w-2 h-2 ${calendarDay?.color || 'bg-gray-300'} rounded-full`}></div>
                  </div>
                  
                  {calendarDay && (
                    <>
                      <div className="flex items-center mb-1">
                        <IconComponent className="w-3 h-3 text-gray-400 mr-1" />
                        <span className="text-xs text-gray-600 truncate">
                          {calendarDay.type}
                        </span>
                      </div>
                      
                      <div className={`text-xs px-2 py-1 rounded ${
                        calendarDay.hasContent 
                          ? 'bg-green-100 text-green-700' 
                          : 'bg-gray-100 text-gray-500'
                      }`}>
                        {calendarDay.hasContent ? '✅ Generated' : 'Not Generated'}
                      </div>
                      
                      {calendarDay.hasContent && calendarDay.content && (
                        <div className="mt-2">
                          <div className="text-xs text-gray-500 overflow-hidden mb-2" style={{
                            display: '-webkit-box',
                            WebkitLineClamp: '2',
                            WebkitBoxOrient: 'vertical'
                          }}>
                            {calendarDay.content.substring(0, 60)}...
                          </div>
                          <div className="text-xs text-amber-600 font-medium">
                            Click to view week →
                          </div>
                        </div>
                      )}
                    </>
                  )}
                </div>
              );
            })}
          </div>
        </div>

        {/* Legend */}
        <div className="mt-8 bg-gray-50 rounded-lg p-6">
          <h3 className="font-semibold text-gray-900 mb-4">Content Types</h3>
          <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-4">
            <div className="flex items-center">
              <div className="w-3 h-3 bg-blue-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Strategic Promo</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-red-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Power Statement</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-green-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Client Experience</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-yellow-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Funny/Relatable</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-purple-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Industry Observation</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-pink-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Client Testimonials</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-indigo-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Engagement Thread</span>
            </div>
            <div className="flex items-center">
              <div className="w-3 h-3 bg-amber-500 rounded-full mr-2"></div>
              <span className="text-sm text-gray-700">Wrap-up Experience</span>
            </div>
          </div>
        </div>
      </div>
    );
  }
  if (currentView === 'dashboard') {
    return (
      <div className="max-w-6xl mx-auto p-6 bg-gray-50 min-h-screen">
        <div className="mb-8">
          <div className="flex items-center mb-4">
            <div className="w-12 h-12 bg-gradient-to-r from-amber-800 to-amber-600 rounded-full flex items-center justify-center mr-4">
              <Heart className="w-6 h-6 text-white" />
            </div>
            <div>
              <h1 className="text-3xl font-bold text-gray-900 mb-1">Batch Bestie</h1>
              <p className="text-sm text-amber-700 font-medium">30-Day Threads Challenge</p>
            </div>
          </div>
          <p className="text-gray-600">Build authority, drive engagement, and attract clients with strategic content</p>
        </div>

        <div className="grid lg:grid-cols-4 gap-6">
          {Object.entries(weeklyFramework).map(([weekNum, week]) => (
            <div key={weekNum} className="bg-white rounded-lg shadow-sm border border-gray-200">
              <div className="p-4 border-b border-gray-200">
                <h3 className="font-semibold text-gray-900">Week {weekNum}</h3>
                <h4 className="text-sm font-medium text-amber-600">{week.title}</h4>
                <p className="text-xs text-gray-500 mt-1">{week.subtitle}</p>
              </div>
              
              <div className="p-4 space-y-2">
                {Object.entries(week.days).map(([day, info]) => (
                  <div key={day} className="flex items-center justify-between p-2 rounded hover:bg-gray-50">
                    <div className="flex items-center">
                      <div className={`w-3 h-3 ${info.color} rounded-full mr-2`}></div>
                      <span className="text-sm text-gray-700">Day {day}</span>
                    </div>
                    <span className="text-xs text-gray-500">{info.type}</span>
                  </div>
                ))}
              </div>

              <div className="p-4 border-t border-gray-200">
                <button
                  onClick={async (e) => {
                    try {
                      e.preventDefault();
                      console.log('Button clicked for week:', weekNum);
                      const weekNumber = parseInt(weekNum);
                      console.log('Parsed week number:', weekNumber);
                      if (isNaN(weekNumber) || weekNumber < 1 || weekNumber > 4) {
                        setUploadError(`Invalid week number: ${weekNum}. Please refresh and try again.`);
                        setTimeout(() => setUploadError(''), 3000);
                        return;
                      }
                      
                      // Check if content exists for this week
                      const weekData = weeklyFramework[weekNumber];
                      const dayNumbers = Object.keys(weekData.days).map(d => parseInt(d));
                      const existingContent = batchContent.filter(item => dayNumbers.includes(item.day));
                      const hasCompleteContent = existingContent.length > 0 && existingContent.every(item => !item.isGenerating && item.content);
                      
                      if (hasCompleteContent) {
                        // Just view existing content
                        console.log('Viewing existing content for week:', weekNumber);
                        setSelectedWeek(weekNumber);
                        setCurrentView('batch');
                      } else {
                        // Generate new content
                        await generateBatchContent(weekNumber);
                      }
                    } catch (error) {
                      console.error('Error in button click handler:', error);
                      setUploadError(`Error: Unable to process Week ${weekNum}. Please try again.`);
                      setTimeout(() => setUploadError(''), 3000);
                    }
                  }}
                  className="w-full bg-gradient-to-r from-amber-800 to-amber-600 text-white py-2 rounded-lg font-medium hover:from-amber-900 hover:to-amber-700 transition-all duration-200"
                >
                  {(() => {
                    const weekNumber = parseInt(weekNum);
                    const weekData = weeklyFramework[weekNumber];
                    if (!weekData) return `Generate Week ${weekNum}`;
                    
                    const dayNumbers = Object.keys(weekData.days).map(d => parseInt(d));
                    const existingContent = batchContent.filter(item => dayNumbers.includes(item.day));
                    const hasCompleteContent = existingContent.length > 0 && existingContent.every(item => !item.isGenerating && item.content);
                    
                    return hasCompleteContent ? `View Week ${weekNum}` : `Generate Week ${weekNum}`;
                  })()}
                </button>
              </div>
            </div>
          ))}
        </div>

        <div className="mt-8 bg-white rounded-lg shadow-sm border border-gray-200 p-6">
          <h2 className="text-xl font-semibold text-gray-900 mb-4">Your Profile</h2>
          <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-4 text-sm">
            <div>
              <span className="font-medium text-gray-600">Industry:</span>
              <p className="text-gray-900">{userProfile.industry || 'Not set'}</p>
            </div>
            <div>
              <span className="font-medium text-gray-600">Voice Style:</span>
              <p className="text-gray-900">{userProfile.voiceStyle || 'Not set'}</p>
            </div>
            <div>
              <span className="font-medium text-gray-600">Personality Themes:</span>
              <p className="text-gray-900">{userProfile.personalityThemes.length > 0 ? userProfile.personalityThemes.join(', ') : 'None selected'}</p>
            </div>
          </div>
        </div>

        <div className="mt-8 bg-white rounded-lg shadow-sm border border-gray-200 p-6">
          <h2 className="text-xl font-semibold text-gray-900 mb-4">Quick Actions</h2>
          <div className="grid md:grid-cols-3 gap-4">
            <button 
              onClick={(e) => {
                try {
                  e.preventDefault();
                  console.log('Edit Profile clicked');
                  setCurrentView('onboarding');
                  setCurrentStep(0);
                } catch (error) {
                  console.error('Error in Edit Profile:', error);
                  setUploadError('Unable to open profile editor. Please refresh the page.');
                  setTimeout(() => setUploadError(''), 3000);
                }
              }}
              className="flex items-center p-4 border border-gray-200 rounded-lg hover:bg-gray-50 transition-colors"
            >
              <Edit3 className="w-5 h-5 text-amber-700 mr-3" />
              <div className="text-left">
                <div className="font-medium">Edit Profile</div>
                <div className="text-sm text-gray-500">Update your voice and brand</div>
              </div>
            </button>
            <div className="relative">
              <button 
                onClick={(e) => {
                  e.preventDefault();
                  setCurrentView('calendar');
                }}
                className="flex items-center p-4 border border-gray-200 rounded-lg hover:bg-gray-50 transition-colors w-full"
              >
                <Calendar className="w-5 h-5 text-amber-600 mr-3" />
                <div className="text-left">
                  <div className="font-medium">View Calendar</div>
                  <div className="text-sm text-gray-500">See your 30-day plan</div>
                </div>
              </button>
            </div>
            <div className="relative">
              <button 
                onClick={handleExportAll}
                className="flex items-center p-4 border border-gray-200 rounded-lg hover:bg-gray-50 transition-colors w-full"
              >
                <Download className="w-5 h-5 text-amber-700 mr-3" />
                <div className="text-left">
                  <div className="font-medium">Export All Generated Content</div>
                  <div className="text-sm text-gray-500">Download your content</div>
                </div>
              </button>
              
              {showExportTooltip && (
                <div className="absolute top-full left-0 right-0 mt-2 p-3 bg-amber-50 border border-amber-200 rounded-lg shadow-lg z-10">
                  <div className="text-sm text-amber-800">
                    <div className="font-medium mb-2">📊 Export Status</div>
                    
                    {/* Week status */}
                    <div className="space-y-1 mb-3">
                      {[1, 2, 3, 4].map(weekNum => {
                        const weekData = weeklyFramework[weekNum];
                        const dayNumbers = Object.keys(weekData.days).map(d => parseInt(d));
                        const weekContent = batchContent.filter(item => 
                          dayNumbers.includes(item.day) && item.content && !item.isGenerating && !item.hasError
                        );
                        const isGenerated = weekContent.length > 0;
                        
                        return (
                          <div key={weekNum} className="flex justify-between text-xs">
                            <span>Week {weekNum}:</span>
                            <span className={isGenerated ? 'text-green-700' : 'text-gray-500'}>
                              {isGenerated ? '✅ Generated' : 'Not yet generated'}
                            </span>
                          </div>
                        );
                      })}
                    </div>
                    
                    <div className="text-xs text-amber-700 mb-2">
                      {batchContent.filter(item => item.content && !item.isGenerating && !item.hasError).length > 0 
                        ? 'CSV will include generated content + placeholders for missing weeks.'
                        : 'No content generated yet. Generate some content first!'
                      }
                    </div>
                    
                    {batchContent.filter(item => item.content && !item.isGenerating && !item.hasError).length > 0 && (
                      <button 
                        onClick={(e) => {
                          e.stopPropagation();
                          doExport();
                          setShowExportTooltip(false);
                        }}
                        className="w-full mt-2 px-3 py-1 bg-amber-600 text-white text-xs rounded hover:bg-amber-700 transition-colors"
                      >
                        Download CSV
                      </button>
                    )}
                  </div>
                  <button 
                    onClick={() => setShowExportTooltip(false)}
                    className="mt-2 text-xs text-amber-600 hover:text-amber-700"
                  >
                    Close ✨
                  </button>
                </div>
              )}
            </div>
          </div>
        </div>
      </div>
    );
  }

  // Remove the separate generating view since we're now showing loading inline
  // Generating View (Loading State) - REMOVED
  // This section was deleted as we now show loading states inline within each thread card

  // Batch Content View
  if (currentView === 'batch') {
    // Filter content to show only the selected week's days
    const currentWeekData = weeklyFramework[selectedWeek];
    const currentWeekDays = Object.keys(currentWeekData?.days || {}).map(d => parseInt(d));
    const currentWeekContent = batchContent
      .filter(thread => currentWeekDays.includes(thread.day))
      .sort((a, b) => a.day - b.day);

    return (
      <div className="max-w-4xl mx-auto p-6 bg-white min-h-screen">
        <div className="flex items-center justify-between mb-8">
          <div className="flex items-center">
            <div className="w-12 h-12 bg-gradient-to-r from-amber-800 to-amber-600 rounded-full flex items-center justify-center mr-4">
              <Heart className="w-6 h-6 text-white" />
            </div>
            <div>
              <h1 className="text-2xl font-bold text-gray-900">Batch Bestie</h1>
              <p className="text-gray-600">Week {selectedWeek} - {weeklyFramework[selectedWeek]?.title}</p>
            </div>
          </div>
          <button
            onClick={() => setCurrentView('dashboard')}
            className="text-amber-700 hover:text-amber-800 font-medium"
          >
            ← Back to Dashboard
          </button>
        </div>

        <div className="space-y-6">
          {currentWeekContent.map((thread, index) => (
            <div key={index} className="border border-gray-200 rounded-lg overflow-hidden">
              <div className="flex items-center justify-between p-4 bg-gray-50 border-b">
                <div className="flex items-center">
                  <div className={`w-4 h-4 ${thread.color} rounded-full mr-3`}></div>
                  <div>
                    <h3 className="font-medium text-gray-900">Day {thread.day}</h3>
                    <p className="text-sm text-gray-500">{thread.type}</p>
                  </div>
                </div>
                <div className="flex items-center space-x-2">
                  {thread.isGenerating ? (
                    <span className="text-xs text-amber-600">Generating...</span>
                  ) : (
                    <>
                      <button
                        onClick={() => copyToClipboard(thread.content)}
                        className="p-2 text-gray-400 hover:text-gray-600 transition-colors"
                        title="Copy to clipboard"
                      >
                        <Copy className="w-4 h-4" />
                      </button>
                      <button 
                        onClick={() => regenerateThread(thread.day)}
                        className="p-2 text-gray-400 hover:text-gray-600 transition-colors"
                        title="Regenerate this thread"
                      >
                        <RefreshCw className="w-4 h-4" />
                      </button>
                    </>
                  )}
                </div>
              </div>
              <div className="p-4">
                {thread.isGenerating ? (
                  <div className="flex items-center justify-center py-8 text-gray-500">
                    <div className="w-6 h-6 border-2 border-amber-600 border-t-transparent rounded-full animate-spin mr-3"></div>
                    Creating your personalized {thread.type.toLowerCase()} thread...
                  </div>
                ) : (
                  <>
                    {thread.content && (
                      <div className="mb-2 text-xs text-gray-500">
                        Character count: {thread.content.length} / 500 recommended
                        {thread.content.length > 500 && thread.content.length <= 1500 && (
                          <span className="text-amber-600 ml-2">Multiple threads recommended</span>
                        )}
                        {thread.content.length > 1500 && (
                          <span className="text-red-600 ml-2">Too long - please regenerate</span>
                        )}
                      </div>
                    )}
                    <pre className="whitespace-pre-wrap text-gray-800 font-sans leading-relaxed">
                      {thread.content || '[Content will appear here once generated]'}
                    </pre>
                  </>
                )}
              </div>
            </div>
          ))}

          {currentWeekContent.length > 0 && !currentWeekContent.some(thread => thread.isGenerating) && (
            <>
              <div className="bg-amber-50 border border-amber-200 rounded-lg p-6 mt-8">
                <h3 className="font-semibold text-amber-900 mb-2">Batch Bestie Pro Tips for This Week:</h3>
                <ul className="text-sm text-amber-800 space-y-1">
                  <li>• Post consistently at the same time each day</li>
                  <li>• Engage with comments within 2 hours of posting</li>
                  <li>• Save high-performing threads for future batches</li>
                  <li>• Track clicks to your {userProfile.trafficDestination} with UTM codes</li>
                </ul>
              </div>

              <div className="flex justify-center space-x-4 mt-8">
                <button
                  onClick={async () => await generateBatchContent(selectedWeek)}
                  className="flex items-center px-6 py-3 bg-gray-100 text-gray-700 rounded-lg font-medium hover:bg-gray-200 transition-colors"
                >
                  <RefreshCw className="w-4 h-4 mr-2" />
                  Regenerate Batch
                </button>
                <button 
                  onClick={() => {
                    try {
                      // Use the same CSV export logic as downloadProfile
                      const csvData = [
                        ['Day', 'Type', 'Content'],
                        ...currentWeekContent.filter(thread => !thread.isGenerating).map(thread => [
                          `Day ${thread.day}`,
                          thread.type,
                          thread.content || ''
                        ])
                      ];

                      // Create CSV content with proper escaping
                      const csvContent = csvData.map(row => 
                        row.map(field => {
                          const cleanField = String(field).replace(/"/g, '""');
                          return `"${cleanField}"`;
                        }).join(',')
                      ).join('\r\n');

                      // Use Blob with explicit type
                      const blob = new Blob(['\uFEFF' + csvContent], { 
                        type: 'text/csv;charset=utf-8;' 
                      });
                      
                      // Create download link
                      const link = document.createElement('a');
                      if (link.download !== undefined) {
                        const url = URL.createObjectURL(blob);
                        link.setAttribute('href', url);
                        link.setAttribute('download', `batch-bestie-week-${selectedWeek}.csv`);
                        link.style.visibility = 'hidden';
                        document.body.appendChild(link);
                        link.click();
                        document.body.removeChild(link);
                        URL.revokeObjectURL(url);
                      }
                    } catch (error) {
                      console.error('Export error:', error);
                      // Fallback: show CSV content in a new window
                      alert('Download failed. Your content will open in a new window - you can copy and save it manually.');
                      const csvData = [
                        ['Day', 'Type', 'Content'],
                        ...currentWeekContent.filter(thread => !thread.isGenerating).map(thread => [
                          `Day ${thread.day}`,
                          thread.type,
                          thread.content || ''
                        ])
                      ];
                      
                      const csvContent = csvData.map(row => 
                        row.map(field => `"${String(field).replace(/"/g, '""')}"`).join(',')
                      ).join('\n');
                      
                      const newWindow = window.open();
                      newWindow.document.write('<pre>' + csvContent + '</pre>');
                    }
                  }}
                  className="flex items-center px-6 py-3 bg-gradient-to-r from-amber-800 to-amber-600 text-white rounded-lg font-medium hover:from-amber-900 hover:to-amber-700 transition-all duration-200"
                >
                  <Download className="w-4 h-4 mr-2" />
                  Export as CSV
                </button>
              </div>
            </>
          )}
        </div>
      </div>
    );
  }

  // Remove the modal components since we're back to tooltips

  // Remove the loading overlay since we're keeping it simple

  return null;
};

export default ThreadsContentSystem;
