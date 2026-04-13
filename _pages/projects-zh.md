---
permalink: /zh/projects/
title: "项目"
author_profile: true
lang: zh
en_url: /projects/
---

<section class="projects-page">
  <div class="projects-hero">
    <div class="projects-eyebrow">精选项目</div>
    <p class="projects-intro">
      我喜欢做那些有真实约束的软件项目：性能、规模、稳定性，以及复杂的边界情况。下面是一些我在后端系统、机器人和底层编程方向上的项目。
    </p>
  </div>

  <div class="projects-grid">
    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://yswcyswc.github.io/posts/2025/8/erininternship/">邮件通知服务（ERIN）</a></h3>
              <a class="project-icon-link" href="https://yswcyswc.github.io/posts/2025/8/erininternship/" aria-label="打开邮件通知服务（ERIN）">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M14 3h7v7h-2V6.41l-9.29 9.3-1.42-1.42 9.3-9.29H14V3zm5 16H5V5h7V3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2v-7h-2v7z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">云系统</span>
          </div>
          <div class="project-stack">
            <span>Node.js</span>
            <span>AWS Lambda</span>
            <span>DynamoDB</span>
            <span>SendGrid</span>
          </div>
          <p class="project-summary">
            为生产环境中的推荐平台构建了无服务器邮件通知系统，使用模块化 AWS Lambda 处理器、SendGrid 模板和多语言支持。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/erin_logo.png" alt="ERIN 项目示意图">
        </div>
      </div>
    </article>

    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://github.com/yswcyswc/Scalable-Robot-Target-Interception-Planner">可扩展机器人目标拦截规划器</a></h3>
              <a class="project-icon-link" href="https://github.com/yswcyswc/Scalable-Robot-Target-Interception-Planner" aria-label="在 GitHub 查看可扩展机器人目标拦截规划器">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 .5C5.65.5.5 5.66.5 12.03c0 5.1 3.3 9.42 7.87 10.95.58.1.79-.25.79-.56v-1.97c-3.2.7-3.88-1.55-3.88-1.55-.53-1.35-1.29-1.72-1.29-1.72-1.05-.72.08-.71.08-.71 1.16.08 1.76 1.2 1.76 1.2 1.03 1.77 2.69 1.26 3.34.97.1-.75.4-1.26.73-1.55-2.55-.29-5.24-1.28-5.24-5.7 0-1.26.45-2.29 1.19-3.09-.12-.29-.52-1.48.11-3.08 0 0 .97-.31 3.18 1.18a10.9 10.9 0 0 1 5.79 0c2.21-1.49 3.17-1.18 3.17-1.18.64 1.6.24 2.79.12 3.08.74.8 1.19 1.83 1.19 3.09 0 4.43-2.7 5.41-5.27 5.69.42.36.79 1.08.79 2.17v3.22c0 .31.21.67.8.56a11.55 11.55 0 0 0 7.86-10.95C23.5 5.66 18.35.5 12 .5z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">机器人</span>
          </div>
          <div class="project-stack">
            <span>C++</span>
            <span>Planning</span>
            <span>Optimization</span>
          </div>
          <p class="project-summary">
            设计了一个机器人规划系统，可在大规模二维加权代价地图上高效完成目标拦截、碰撞检测和轨迹对齐。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/target_interception.png" alt="目标拦截规划器示意图">
        </div>
      </div>
    </article>

    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://github.com/yswcyswc/Order-Management-System">订单管理系统</a></h3>
              <a class="project-icon-link" href="https://github.com/yswcyswc/Order-Management-System" aria-label="在 GitHub 查看订单管理系统">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 .5C5.65.5.5 5.66.5 12.03c0 5.1 3.3 9.42 7.87 10.95.58.1.79-.25.79-.56v-1.97c-3.2.7-3.88-1.55-3.88-1.55-.53-1.35-1.29-1.72-1.29-1.72-1.05-.72.08-.71.08-.71 1.16.08 1.76 1.2 1.76 1.2 1.03 1.77 2.69 1.26 3.34.97.1-.75.4-1.26.73-1.55-2.55-.29-5.24-1.28-5.24-5.7 0-1.26.45-2.29 1.19-3.09-.12-.29-.52-1.48.11-3.08 0 0 .97-.31 3.18 1.18a10.9 10.9 0 0 1 5.79 0c2.21-1.49 3.17-1.18 3.17-1.18.64 1.6.24 2.79.12 3.08.74.8 1.19 1.83 1.19 3.09 0 4.43-2.7 5.41-5.27 5.69.42.36.79 1.08.79 2.17v3.22c0 .31.21.67.8.56a11.55 11.55 0 0 0 7.86-10.95C23.5 5.66 18.35.5 12 .5z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">全栈</span>
          </div>
          <div class="project-stack">
            <span>Ruby on Rails</span>
            <span>React</span>
            <span>REST APIs</span>
          </div>
          <p class="project-summary">
            开发了一个全栈订单管理应用，包含 Rails 后端、React 前端、基于角色的访问控制，以及面向关系型数据库的 REST API。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/rails_arch.png" alt="订单管理系统架构图">
        </div>
      </div>
    </article>

    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://github.com/yswcyswc/Sampling-Based-Planning-in-Manipulator-Configuration-Space">高自由度机械臂运动规划</a></h3>
              <a class="project-icon-link" href="https://github.com/yswcyswc/Sampling-Based-Planning-in-Manipulator-Configuration-Space" aria-label="在 GitHub 查看高自由度机械臂运动规划">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 .5C5.65.5.5 5.66.5 12.03c0 5.1 3.3 9.42 7.87 10.95.58.1.79-.25.79-.56v-1.97c-3.2.7-3.88-1.55-3.88-1.55-.53-1.35-1.29-1.72-1.29-1.72-1.05-.72.08-.71.08-.71 1.16.08 1.76 1.2 1.76 1.2 1.03 1.77 2.69 1.26 3.34.97.1-.75.4-1.26.73-1.55-2.55-.29-5.24-1.28-5.24-5.7 0-1.26.45-2.29 1.19-3.09-.12-.29-.52-1.48.11-3.08 0 0 .97-.31 3.18 1.18a10.9 10.9 0 0 1 5.79 0c2.21-1.49 3.17-1.18 3.17-1.18.64 1.6.24 2.79.12 3.08.74.8 1.19 1.83 1.19 3.09 0 4.43-2.7 5.41-5.27 5.69.42.36.79 1.08.79 2.17v3.22c0 .31.21.67.8.56a11.55 11.55 0 0 0 7.86-10.95C23.5 5.66 18.35.5 12 .5z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">算法</span>
          </div>
          <div class="project-stack">
            <span>C++</span>
            <span>RRT-Connect</span>
            <span>PRM</span>
          </div>
          <p class="project-summary">
            实现了基于采样的运动规划算法，为高自由度平面机械臂生成无碰撞轨迹。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/dofarms.png" alt="高自由度机械臂规划示意图">
        </div>
      </div>
    </article>

    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://yswcyswc.github.io/posts/2024/8/icsproject/">动态内存分配器</a></h3>
              <a class="project-icon-link" href="https://yswcyswc.github.io/posts/2024/8/icsproject/" aria-label="打开动态内存分配器">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M14 3h7v7h-2V6.41l-9.29 9.3-1.42-1.42 9.3-9.29H14V3zm5 16H5V5h7V3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2v-7h-2v7z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">系统</span>
          </div>
          <div class="project-stack">
            <span>C</span>
            <span>Memory Management</span>
            <span>Performance</span>
          </div>
          <p class="project-summary">
            用 C 实现了一个动态内存分配器，支持 <code>malloc</code>、<code>free</code>、<code>realloc</code> 和 <code>calloc</code>，并提升了内存利用率。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/dynam.png" alt="动态内存分配器示意图">
        </div>
      </div>
    </article>

    <article class="project-card">
      <div class="project-card-inner">
        <div class="project-card-content">
          <div class="project-header">
            <div class="project-heading">
              <h3 class="project-title"><a href="https://github.com/yswcyswc/15112TermProject">MazeRunner</a></h3>
              <a class="project-icon-link" href="https://github.com/yswcyswc/15112TermProject" aria-label="在 GitHub 查看 MazeRunner">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path d="M12 .5C5.65.5.5 5.66.5 12.03c0 5.1 3.3 9.42 7.87 10.95.58.1.79-.25.79-.56v-1.97c-3.2.7-3.88-1.55-3.88-1.55-.53-1.35-1.29-1.72-1.29-1.72-1.05-.72.08-.71.08-.71 1.16.08 1.76 1.2 1.76 1.2 1.03 1.77 2.69 1.26 3.34.97.1-.75.4-1.26.73-1.55-2.55-.29-5.24-1.28-5.24-5.7 0-1.26.45-2.29 1.19-3.09-.12-.29-.52-1.48.11-3.08 0 0 .97-.31 3.18 1.18a10.9 10.9 0 0 1 5.79 0c2.21-1.49 3.17-1.18 3.17-1.18.64 1.6.24 2.79.12 3.08.74.8 1.19 1.83 1.19 3.09 0 4.43-2.7 5.41-5.27 5.69.42.36.79 1.08.79 2.17v3.22c0 .31.21.67.8.56a11.55 11.55 0 0 0 7.86-10.95C23.5 5.66 18.35.5 12 .5z"></path>
                </svg>
              </a>
            </div>
            <span class="project-badge">交互式</span>
          </div>
          <div class="project-stack">
            <span>Python</span>
            <span>Game Design</span>
            <span>Procedural Generation</span>
          </div>
          <p class="project-summary">
            用 Python 构建了一个迷宫探索游戏，采用 MVC 风格结构，并通过程序化生成提供不同布局和难度。
          </p>
        </div>
        <div class="project-visual">
          <img class="project-image" src="/assets/images/mazenav.png" alt="MazeRunner 游戏画面">
        </div>
      </div>
    </article>
  </div>
</section>
