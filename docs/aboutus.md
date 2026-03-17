---
layout: default
title: About Us
permalink: /about/
description: "Meet the staff and board of the NJ Civic Information Consortium, a first-of-its-kind initiative supporting local news in New Jersey."
---

<section class="page-hero page-hero-about-us">
  <div class="page-hero-content">
    <span class="hero-label">Our Organization</span>
    <h1>About Us</h1>
    <p class="page-hero-subtitle">Learn about the people and institutions behind the New Jersey Civic Information Consortium.</p>
  </div>
</section>

<section class="content-section">
  <div class="content-container">
    <div class="content-grid content-grid-reverse">
      <div class="content-text">
        <span class="section-label">Our Story</span>
        <h2>A First-of-Its-Kind Initiative</h2>
        <p>The New Jersey Civic Information Consortium was created by the State of New Jersey in 2018 to address the local news crisis. It emerged from a broad stakeholder coalition led by Free Press, building on prior initiatives by the Geraldine R. Dodge Foundation and Montclair State University's Center for Cooperative Media.</p>
        <p>Six public universities partner in this effort: The College of New Jersey, Montclair State University, NJIT, Rowan University, Kean University, and Rutgers University. Montclair State serves as the host institution.</p>
        <p>A 16-member Board of Directors governs the Consortium's operations, and state law prevents New Jersey and the Consortium from owning funded projects or exercising editorial control.</p>
        <div class="video-embed-wrapper" style="margin-top: 32px;">
          <iframe src="https://www.youtube.com/embed/zbSi8QIEhTM" title="NJ Civic Information Consortium" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen class="video-embed"></iframe>
        </div>
      </div>
      <div class="content-feature">
        <div class="vision-card">
          <h3>Our Mission</h3>
          <p>The Consortium provides financial resources to support and grow trustworthy, community-based news and information sources throughout New Jersey.</p>
        </div>
        <div class="vision-card" style="margin-top: 24px;">
          <h3>Our Vision</h3>
          <p>We envision a future in which every person in New Jersey has access to thriving, representative local news and information that enhances their lives and communities.</p>
        </div>
      </div>
    </div>
  </div>
</section>

<section class="content-section bg-offwhite">
  <div class="content-container">
    <div class="section-header">
      <span class="section-label">Our Team</span>
      <h2>Staff</h2>
    </div>

    <div class="team-grid">
      {% for member in site.data.staff %}
      <div class="team-card">
        <div class="team-photo">
          <img src="{{ ‘/assets/images/’ | append: member.photo | relative_url }}" alt="{{ member.name }}">
        </div>
        <h3>{{ member.name }}</h3>
        <span class="team-title">{{ member.title }}</span>
        <div class="expand-item">
          <button class="expand-toggle" aria-expanded="false">
            <div class="expand-header"><span>View Bio</span></div>
            <span class="expand-icon">+</span>
          </button>
          <div class="expand-content">
            <p>{{ member.bio }}</p>
          </div>
        </div>
      </div>
      {% endfor %}
    </div>
  </div>
</section>

<section class="content-section">
  <div class="content-container">
    <div class="section-header">
      <span class="section-label">Governance</span>
      <h2>Board of Directors</h2>
      <p>Our 16-member board includes representatives from partner universities, community organizations, and the public.</p>
    </div>

    <div class="board-grid">
      {% for member in site.data.board %}
      <div class="board-member">
        <span class="board-name">{{ member.name }}</span>
        <span class="board-title">{{ member.title }}</span>
        {% if member.role %}<span class="board-title">{{ member.role }}</span>{% endif %}
        <span class="board-appointment">{{ member.appointment }}</span>
      </div>
      {% endfor %}
    </div>
  </div>
</section>

<section class="content-section bg-offwhite">
  <div class="content-container">
    <div class="section-header">
      <span class="section-label">Partners</span>
      <h2>Member Universities</h2>
      <p>Six public universities partner with the Consortium to strengthen local news across New Jersey.</p>
    </div>
    <div class="university-grid">
      <div class="university-item">The College of New Jersey</div>
      <div class="university-item">Montclair State University</div>
      <div class="university-item">New Jersey Institute of Technology</div>
      <div class="university-item">Rowan University</div>
      <div class="university-item">Kean University</div>
      <div class="university-item">Rutgers University</div>
    </div>

    <div class="section-header" style="margin-top: 64px;">
      <h2>Funders</h2>
    </div>
    <div class="university-grid">
      <div class="university-item">The State of New Jersey</div>
      <div class="university-item">Robert Wood Johnson Foundation</div>
      <div class="university-item">Democracy Fund</div>
      <div class="university-item">Press Forward</div>
      <div class="university-item">F.M. Kirby Foundation</div>
      <div class="university-item">Community Foundation of New Jersey</div>
      <div class="university-item">Grunin Foundation</div>
      <div class="university-item">EQUIP NJ</div>
    </div>

    <div class="section-header" style="margin-top: 64px;">
      <h2>Collaborative Partners</h2>
    </div>
    <div class="university-grid">
      <div class="university-item">Free Press</div>
      <div class="university-item">Hoboken Strategy Group</div>
      <div class="university-item">Center for Cooperative Media at MSU</div>
      <div class="university-item">Community Foundation of South Jersey</div>
      <div class="university-item">Blue Engine Collaborative</div>
      <div class="university-item">City Bureau</div>
      <div class="university-item">Rebuild Local News</div>
    </div>
  </div>
</section>

<section class="cta-section">
  <div class="cta-container">
    <h2>Get in Touch</h2>
    <p>Have questions or want to partner? We'd love to hear from you.</p>
    <div class="cta-buttons">
      <a href="mailto:info@njcivicinfo.org" class="btn btn-primary">Email Us</a>
    </div>
  </div>
</section>
