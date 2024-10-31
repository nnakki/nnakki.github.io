---
title: "Hi there! I'm Yezi👋🏻"
permalink: /about/
layout: single
comments: false
---
<html lang="ko">
<head>
  <style>
    body {
      font-family: $global-font-family;
      line-height: 1.6;
      margin: 0;
      padding: 0;
      color: #333;
    }
    .container {
      max-width: 800px;
      margin: auto;
      padding: 20px;
    }
    .profile-pic {
      width: 400px;
      height: 400px;
      /* border-radius: 50%; */
      object-fit: cover;
      /* margin-bottom: 20px; */
    }
    h1, h2 {
      color: rgb(133.34,162.74,196.06);
    }
    .section {
      /* margin-bottom: 40px; */
    }
    .addLine{
        border-left: 2px solid rgba(199, 198, 198, 0.7); 
        margin: 0.5em 0 0 0.5em; 
        padding-left: 1.5em; 
        font-weight: 500;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- 프로필 섹션 -->
    <div class="section" id="profile">
      <img src="../assets/images/about.png" alt="Profile Picture" class="profile-pic">
    </div>
    <div class="section" id="tech-stack">
          <h2><i class="fas fa-user-edit" aria-hidden="true"></i>&nbsp;&nbsp;Info</h2>
    </div>
    <div class="addLine">
    <ul class="author__urls social-icons">
        <li itemprop="homeLocation" itemscope itemtype="https://schema.org/Place">
          <i class="fas fa-fw fa-map-marker-alt" aria-hidden="true"></i> <span itemprop="name">  Seoul, Korea</span>
        </li>
        <li>
          <a href="https://github.com/nnakki" itemprop="sameAs" rel="nofollow noopener noreferrer">
            <i class="fab fa-fw fa-github" aria-hidden="true"></i><span class="label">  https://github.com/nnakki</span>
          </a>
        </li>
        <li>
          <a href="mailto:yvely225@gmail.com">
            <meta itemprop="email" content="yvely225@gmail.com" />
            <i class="fas fa-fw fa-envelope-square" aria-hidden="true"></i><span class="label">  yvely225@gmail.com</span>
          </a>
        </li>
        <li>
          <a href="https://www.instagram.com/likedbyezi_/" itemprop="sameAs" rel="nofollow noopener noreferrer">
            <i class="fab fa-fw fa-instagram" aria-hidden="true"></i><span class="label">  https://www.instagram.com/likedbyezi_/</span>
          </a>
        </li>
    </ul>
    </div>
    <!-- 기술 스택 섹션 -->
    <div class="section">
        <h2><i class="fas fa-tools" aria-hidden="true"></i>&nbsp;&nbsp;Tech Skills</h2>
      <ul class="addLine author__urls social-icons" >
        <li><strong>언어:</strong> Java, Kotlin, JavaScript</li>
        <li><strong>프레임워크:</strong> Spring Boot</li>
        <li><strong>데이터베이스:</strong> MySQL, PostgreSQL</li>
        <li><strong>기타:</strong> Git, Docker, Kubernetes, AWS</li>
      </ul>
    </div>
    <!-- 경력 섹션
    <div class="section" id="experience">
      <h2><i class="fas fa-building" aria-hidden="true"></i>&nbsp;&nbsp;Career</h2>
      <h3>회사 이름 - 직무 (YYYY-MM ~ YYYY-MM)</h3>
      <p>여기에서 수행한 주요 업무 및 프로젝트 경험에 대해 설명합니다. 예를 들어, 시스템 최적화, 백엔드 API 개발 등을 수행했다고 기재할 수 있습니다.</p>
    </div> -->
    <!-- 프로젝트 섹션 
    <div class="section" id="projects">
        <h2><i class="fas fa-project-diagram" aria-hidden="true"></i>&nbsp;&nbsp;Projects</h2>
      <ul>
        <li><strong>프로젝트 이름:</strong> 프로젝트 설명. <a href="프로젝트 링크">자세히 보기</a></li>
        <li><strong>프로젝트 이름:</strong> 프로젝트 설명. <a href="프로젝트 링크">자세히 보기</a></li>
      </ul>
    </div> -->
  </div>
</body>
</html>
