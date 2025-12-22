The glasslike effect in your CookieConsentBanner component is achieved through a combination of CSS backdrop filters and layered styling. Here's how it works:

## Key Elements Creating the Glass Effect:

1. **Backdrop Blur Layer**:

- `backdrop-blur-sm` applies a subtle blur to everything behind the element
- `bg-black/5` adds a very light black tint (5% opacity)
- `absolute inset-0` makes it cover the entire banner area

2. **Semi-transparent Background**:

- `from-white via-white to-primary/5` creates a gradient that's mostly white but slightly tinted
- `border-primary/20` adds a subtle colored border
- `shadow-2xl` provides depth

3. **Layering Structure**:

- The backdrop blur sits behind the content (`absolute`)
- The main content sits on top (`relative`) with its own background
- This creates the classic "frosted glass" effect where you can see through but with distortion

## The Effect Breakdown:

- **Transparency**: Semi-transparent backgrounds allow underlying content to show through
- **Blur**: `backdrop-blur-sm` creates the frosted/etched glass appearance
- **Subtle Tinting**: Light overlays (`bg-black/5`, `to-primary/5`) add depth
- **Shadows**: `shadow-2xl` lifts the element off the page
- **Gradients**: Create smooth color transitions typical of glass materials

This technique is commonly called "glassmorphism" and works best when there's content behind the glass element to blur and show through.

```tsx
// components/GeneralComponents/CookieConsentBanner.tsx
"use client";
import React, { useState, useEffect } from "react";

const CookieConsentBanner: React.FC = () => {
  const [showBanner, setShowBanner] = useState(false);
  const [isVisible, setIsVisible] = useState(false);

  useEffect(() => {
    const consent = localStorage.getItem("cookie_consent");
    if (!consent) {
      setShowBanner(true);
      // Add a small delay for smooth animation
      setTimeout(() => setIsVisible(true), 100);
    }
  }, []);

  const handleAccept = () => {
    localStorage.setItem("cookie_consent", "accepted");
    setIsVisible(false);
    setTimeout(() => setShowBanner(false), 300); // Wait for animation to complete
  };

  const handleDecline = () => {
    localStorage.setItem("cookie_consent", "declined");
    setIsVisible(false);
    setTimeout(() => setShowBanner(false), 300);
    // Here you might disable non-essential cookies or tracking
    console.log("User declined cookies. Non-essential cookies disabled.");
  };

  const handleLearnMore = () => {
    window.open("/privacy-policy", "_blank");
  };

  if (!showBanner) return null;

  return (
    <div
      className={`fixed bottom-0 left-0 right-0 z-50 transform transition-all duration-300 ease-in-out ${
        isVisible ? "translate-y-0 opacity-100" : "translate-y-full opacity-0"
      }`}
    >
      {/* Backdrop blur */}
      <div className="absolute inset-0 bg-black/5 backdrop-blur-sm"></div>
      
      <div className="relative bg-gradient-to-r from-white via-white to-primary/5 border-t border-primary/20 shadow-2xl">
        <div className="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
          <div className="flex flex-col lg:flex-row items-start lg:items-center justify-between gap-6">
            {/* Content */}
            <div className="flex-1 min-w-0">
              <div className="flex items-start gap-4">
                {/* Enhanced Cookie Icon with Wave Pattern */}
                <div className="flex-shrink-0 mt-1">
                  <div className="relative w-10 h-10 bg-gradient-to-br from-primary to-primary/80 rounded-xl flex items-center justify-center shadow-lg">
                    <svg
                      className="w-5 h-5 text-white"
                      fill="currentColor"
                      viewBox="0 0 24 24"
                    >
                      <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z"/>
                    </svg>
                    {/* Wavelink accent */}
                    <div className="absolute -top-1 -right-1 w-3 h-3 bg-secondary rounded-full border-2 border-white"></div>
                  </div>
                </div>
                
                {/* Enhanced Text Content */}
                <div className="flex-1">
                  <h3 className="text-lg font-bold text-neutral mb-2 flex items-center gap-2">
                    <span>Cookie Preferences</span>
                  </h3>
                  <p className="text-sm text-neutral-700 leading-relaxed mb-2">
                    We use cookies to enhance your experience and provide personalized content. 
                    This helps us improve our internet services and ensure you get the best connection experience.
                  </p>
                  <div className="flex items-center gap-4 text-xs text-neutral-500">
                    <span className="flex items-center gap-1">
                      <span className="w-2 h-2 bg-green-500 rounded-full"></span>
                      Essential cookies (always active)
                    </span>
                    <span className="flex items-center gap-1">
                      <span className="w-2 h-2 bg-blue-500 rounded-full"></span>
                      Analytics & Performance
                    </span>
                  </div>
                </div>
              </div>
            </div>

            {/* Enhanced Actions */}
            <div className="flex flex-col sm:flex-row gap-3 w-full lg:w-auto lg:min-w-fit">
              <button
                onClick={handleLearnMore}
                className="btn btn-ghost btn-sm text-primary hover:bg-primary/10 border border-primary/20 order-3 sm:order-1"
              >
                <svg className="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                Learn More
              </button>
              <button
                onClick={handleDecline}
                className="btn btn-outline btn-sm text-neutral-600 hover:bg-base-200 border-neutral-300 order-2"
              >
                <svg className="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728L5.636 5.636m12.728 12.728L18.364 5.636M5.636 18.364l12.728-12.728" />
                </svg>
                Decline Optional
              </button>
              <button
                onClick={handleAccept}
                className="btn btn-primary btn-sm text-white shadow-lg hover:shadow-xl transform hover:scale-105 transition-all order-1 sm:order-3"
              >
                <svg className="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M5 13l4 4L19 7" />
                </svg>
                Accept All
              </button>
            </div>
          </div>

          {/* Animated Progress Bar */}
          <div className="mt-4 hidden sm:block">
            <div className="w-full bg-base-200/50 rounded-full h-1 overflow-hidden">
              <div 
                className="bg-gradient-to-r from-primary to-secondary h-1 rounded-full transition-all duration-1000 ease-out transform"
                style={{ 
                  width: isVisible ? "100%" : "0%",
                  transform: isVisible ? "translateX(0)" : "translateX(-100%)"
                }}
              ></div>
            </div>
            <p className="text-xs text-neutral-400 mt-1 text-center">
              Your privacy matters to us • Wavelink Internet Services
            </p>
          </div>
        </div>
      </div>
    </div>
  );
};

export default CookieConsentBanner;

```