<!DOCTYPE html>
<html lang='en'>
<head prefix='og: http://ogp.me/ns#'>
<meta charset='utf-8'>
<meta content='IE=edge' http-equiv='X-UA-Compatible'>
<meta content='object' property='og:type'>
<meta content='GitLab' property='og:site_name'>
<meta content='Running-Mastodon/Production-guide.md · 51f4311fb3451260e888b54ace8e17e8ed9141e1 · Greg / documentation' property='og:title'>
<meta content='Full documentation repository for Mastodon' property='og:description'>
<meta content='https://gitlab.komula.ninja/assets/gitlab_logo-cdf021b35c4e6bb149e26460f26fae81e80552bc879179dd80e9e9266b14e894.png' property='og:image'>
<meta content='https://gitlab.komula.ninja/G_Dog1985/documentation/blob/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md' property='og:url'>
<meta content='summary' property='twitter:card'>
<meta content='Running-Mastodon/Production-guide.md · 51f4311fb3451260e888b54ace8e17e8ed9141e1 · Greg / documentation' property='twitter:title'>
<meta content='Full documentation repository for Mastodon' property='twitter:description'>
<meta content='https://gitlab.komula.ninja/assets/gitlab_logo-cdf021b35c4e6bb149e26460f26fae81e80552bc879179dd80e9e9266b14e894.png' property='twitter:image'>

<title>Running-Mastodon/Production-guide.md · 51f4311fb3451260e888b54ace8e17e8ed9141e1 · Greg / documentation · GitLab</title>
<meta content='Full documentation repository for Mastodon' name='description'>
<link rel="shortcut icon" type="image/x-icon" href="/assets/favicon-075eba76312e8421991a0c1f89a89ee81678bcde72319dd3e8047e2a47cd3a42.ico" />
<link rel="stylesheet" media="all" href="/assets/application-59a9f3663ed17a9a3d64f061578f7b30e0511a59a14b8a22841f2ee72033778a.css" />
<link rel="stylesheet" media="print" href="/assets/print-68eed6d8135d858318821e790e25da27b2b4b9b8dbb1993fa6765d8e2e3e16ee.css" />
<script src="/assets/application-4f2bb3ce85aafe038f185b261e0d128576ccfbfacbbed3ac23dacc48e7104818.js"></script>
<meta name="csrf-param" content="authenticity_token" />
<meta name="csrf-token" content="B6h3S7dNYvIoygqCu6A87lRg85wA/jzjHxH1Ca6f4PDwgyfpKMS5EEDesSbyGauDVOWbc5Q+vHrTHSTAZTMw1g==" />
<script>
//<![CDATA[
window.gon={};gon.api_version="v3";gon.default_avatar_url="http://gitlab.komula.ninja/assets/no_avatar-07eeb128b993e74003e8664cff0b8e1e7234cec0443766a6763df32ca3472c23.png";gon.default_issues_tracker="gitlab";gon.max_file_size=4096;gon.relative_url_root="";gon.shortcuts_path="/help/shortcuts";gon.user_color_scheme="white";
//]]>
</script>
<meta content='origin-when-cross-origin' name='referrer'>
<meta content='width=device-width, initial-scale=1, maximum-scale=1' name='viewport'>
<meta content='#474D57' name='theme-color'>
<link rel="apple-touch-icon" type="image/x-icon" href="/assets/touch-icon-iphone-2d64ecc33893baab71adc15ff19a803a59911cc2651fb9d4d62af1379ff89aab.png" />
<link rel="apple-touch-icon" type="image/x-icon" href="/assets/touch-icon-ipad-d08897d57e1bc9400024ac15465340e832a8e7b166b91624138d48ea2c739596.png" sizes="76x76" />
<link rel="apple-touch-icon" type="image/x-icon" href="/assets/touch-icon-iphone-retina-81446c57f3351d1dacd0fb5f23ced74ba63d3878810bedea343999c6a12b3915.png" sizes="120x120" />
<link rel="apple-touch-icon" type="image/x-icon" href="/assets/touch-icon-ipad-retina-e2a776da039936ac240e76615add47b25ab77c75a5fa2d1b3907f83d5502b911.png" sizes="152x152" />
<link color='rgb(226, 67, 41)' href='/assets/logo-d36b5212042cebc89b96df4bf6ac24e43db316143e89926c0db839ff694d2de4.svg' rel='mask-icon'>
<meta content='/assets/msapplication-tile-49c9c46afd2ab9bbf35e69138bc62f8c31fa55584bd494956ac6e58e6aadc813.png' name='msapplication-TileImage'>
<meta content='#30353E' name='msapplication-TileColor'>




<style>
  [data-user-is] {
    display: none !important;
  }
  
  [data-user-is=""] {
    display: block !important;
  }
  
  [data-user-is=""][data-display="inline"] {
    display: inline !important;
  }
  
  [data-user-is-not] {
    display: block !important;
  }
  
  [data-user-is-not][data-display="inline"] {
    display: inline !important;
  }
  
  [data-user-is-not=""] {
    display: none !important;
  }
</style>

</head>

<body class='ui_charcoal' data-page='projects:blob:show'>

<header class='header-expanded navbar navbar-fixed-top navbar-gitlab'>
<div class='container-fluid'>
<div class='header-content'>
<button class='side-nav-toggle' type='button'>
<span class='sr-only'>Toggle navigation</span>
<i class="fa fa-bars"></i>
</button>
<button class='navbar-toggle' type='button'>
<span class='sr-only'>Toggle navigation</span>
<i class="fa fa-angle-left"></i>
</button>
<div class='navbar-collapse collapse'>
<ul class='nav navbar-nav'>
<li class='hidden-sm hidden-xs'>
<div class='has-location-badge search search-form'>
<form class="navbar-form" action="/search" accept-charset="UTF-8" method="get"><input name="utf8" type="hidden" value="&#x2713;" />
<div class='search-input-container'>
<div class='search-location-badge'>
<span class='location-badge'>
<i class='location-text'>
This project
</i>
</span>
</div>
<div class='search-input-wrap'>
<div class='dropdown' data-url='/search/autocomplete'>
<input type="search" name="search" id="search" placeholder="Search" class="search-input dropdown-menu-toggle" spellcheck="false" tabindex="1" autocomplete="off" data-toggle="dropdown" />
<div class='dropdown-menu dropdown-select'>
<div class="dropdown-content"><ul>
<li>
<a class='is-focused dropdown-menu-empty-link'>
Loading...
</a>
</li>
</ul>
</div><div class="dropdown-loading"><i class="fa fa-spinner fa-spin"></i></div>
</div>
<i class='search-icon'></i>
<i class='clear-icon js-clear-input'></i>
</div>
</div>
</div>
<input type="hidden" name="group_id" id="group_id" />
<input type="hidden" name="project_id" id="search_project_id" value="31" />
<input type="hidden" name="search_code" id="search_code" value="true" />
<input type="hidden" name="repository_ref" id="repository_ref" value="51f4311fb3451260e888b54ace8e17e8ed9141e1" />

<div class='search-autocomplete-opts hide' data-autocomplete-path='/search/autocomplete' data-autocomplete-project-id='31' data-autocomplete-project-ref='51f4311fb3451260e888b54ace8e17e8ed9141e1'></div>
</form>

</div>

</li>
<li class='visible-sm visible-xs'>
<a title="Search" data-toggle="tooltip" data-placement="bottom" data-container="body" href="/search"><i class="fa fa-search"></i>
</a></li>
<li>
<div>
<a class="btn btn-sign-in btn-success" href="/users/sign_in?redirect_to_referer=yes">Sign in</a>
</div>
</li>
</ul>
</div>
<h1 class='title'><a href="/u/G_Dog1985">Greg</a> / <a class="project-item-select-holder" href="/G_Dog1985/documentation">documentation</a> &middot; <a href="/G_Dog1985/documentation/tree/51f4311fb3451260e888b54ace8e17e8ed9141e1">Files</a></h1>
<div class='js-dropdown-menu-projects'>
<div class='dropdown-menu dropdown-select dropdown-menu-projects'>
<div class="dropdown-title"><span>Go to a project</span><button class="dropdown-title-button dropdown-menu-close" aria-label="Close" type="button"><i class="fa fa-times dropdown-menu-close-icon"></i></button></div>
<div class="dropdown-input"><input type="search" id="" class="dropdown-input-field" placeholder="Search your projects" /><i class="fa fa-search dropdown-input-search"></i><i role="button" class="fa fa-times dropdown-input-clear js-dropdown-input-clear"></i></div>
<div class="dropdown-content"></div>
<div class="dropdown-loading"><i class="fa fa-spinner fa-spin"></i></div>
</div>
</div>

</div>
</div>
</header>

<script>
  var findFileURL = "/G_Dog1985/documentation/find_file/51f4311fb3451260e888b54ace8e17e8ed9141e1";
</script>

<div class='page-sidebar-expanded page-with-sidebar'>


<div class='nicescroll sidebar-expanded sidebar-wrapper'>
<div class='header-logo'>
<a id='logo'>
<svg width="36" height="36" id="tanuki-logo">
  <path id="tanuki-right-ear" class="tanuki-shape" fill="#e24329" d="M2 14l9.38 9v-9l-4-12.28c-.205-.632-1.176-.632-1.38 0z"/>
  <path id="tanuki-left-ear" class="tanuki-shape" fill="#e24329" d="M34 14l-9.38 9v-9l4-12.28c.205-.632 1.176-.632 1.38 0z"/>
  <path id="tanuki-nose" class="tanuki-shape" fill="#e24329" d="M18,34.38 3,14 33,14 Z"/>
  <path id="tanuki-right-eye" class="tanuki-shape" fill="#fc6d26" d="M18,34.38 11.38,14 2,14 6,25Z"/>
  <path id="tanuki-left-eye" class="tanuki-shape" fill="#fc6d26" d="M18,34.38 24.62,14 34,14 30,25Z"/>
  <path id="tanuki-right-cheek" class="tanuki-shape" fill="#fca326" d="M2 14L.1 20.16c-.18.565 0 1.2.5 1.56l17.42 12.66z"/>
  <path id="tanuki-left-cheek" class="tanuki-shape" fill="#fca326" d="M34 14l1.9 6.16c.18.565 0 1.2-.5 1.56L18 34.38z"/>
</svg>

</a>
<a class="gitlab-text-container-link" title="Dashboard" id="js-shortcuts-home" href="/"><div class='gitlab-text-container'>
<h3>GitLab</h3>
</div>
</a></div>
<ul class='nav nav-sidebar'>
<li class=""><a title="Go to dashboard" class="back-link" href="/"><i class="fa fa-caret-square-o-left fa-fw"></i>
<span>
Go to dashboard
</span>
</a></li><li class='separate-item'></li>
<li class="home"><a title="Project" class="shortcuts-project" href="/G_Dog1985/documentation"><i class="fa fa-bookmark fa-fw"></i>
<span>
Project
</span>
</a></li><li class=""><a title="Activity" class="shortcuts-project-activity" href="/G_Dog1985/documentation/activity"><i class="fa fa-dashboard fa-fw"></i>
<span>
Activity
</span>
</a></li><li class="active"><a title="Files" class="shortcuts-tree" href="/G_Dog1985/documentation/tree/51f4311fb3451260e888b54ace8e17e8ed9141e1"><i class="fa fa-files-o fa-fw"></i>
<span>
Files
</span>
</a></li><li class=""><a title="Commits" class="shortcuts-commits" href="/G_Dog1985/documentation/commits/51f4311fb3451260e888b54ace8e17e8ed9141e1"><i class="fa fa-history fa-fw"></i>
<span>
Commits
</span>
</a></li><li class=""><a title="Builds" class="shortcuts-builds" href="/G_Dog1985/documentation/builds"><i class="fa fa-cubes fa-fw"></i>
<span>
Builds
<span class='count builds_counter'>0</span>
</span>
</a></li><li class=""><a title="Graphs" class="shortcuts-graphs" href="/G_Dog1985/documentation/graphs/51f4311fb3451260e888b54ace8e17e8ed9141e1"><i class="fa fa-area-chart fa-fw"></i>
<span>
Graphs
</span>
</a></li><li class=""><a title="Milestones" href="/G_Dog1985/documentation/milestones"><i class="fa fa-clock-o fa-fw"></i>
<span>
Milestones
</span>
</a></li><li class=""><a title="Issues" class="shortcuts-issues" href="/G_Dog1985/documentation/issues"><i class="fa fa-exclamation-circle fa-fw"></i>
<span>
Issues
<span class='count issue_counter'>0</span>
</span>
</a></li><li class=""><a title="Merge Requests" class="shortcuts-merge_requests" href="/G_Dog1985/documentation/merge_requests"><i class="fa fa-tasks fa-fw"></i>
<span>
Merge Requests
<span class='count merge_counter'>0</span>
</span>
</a></li><li class=""><a title="Members" class="team-tab tab" href="/G_Dog1985/documentation/project_members"><i class="fa fa-users fa-fw"></i>
<span>
Members
</span>
</a></li><li class=""><a title="Labels" href="/G_Dog1985/documentation/labels"><i class="fa fa-tags fa-fw"></i>
<span>
Labels
</span>
</a></li><li class=""><a title="Wiki" class="shortcuts-wiki" href="/G_Dog1985/documentation/wikis/home"><i class="fa fa-book fa-fw"></i>
<span>
Wiki
</span>
</a></li><li class=""><a title="Forks" href="/G_Dog1985/documentation/forks"><i class="fa fa-code-fork fa-fw"></i>
<span>
Forks
</span>
</a></li><li class='hidden'>
<a title="Network" class="shortcuts-network" href="/G_Dog1985/documentation/network/51f4311fb3451260e888b54ace8e17e8ed9141e1">Network
</a></li>
<li class='hidden'>
<a class="shortcuts-new-issue" href="/G_Dog1985/documentation/issues/new">Create a new issue
</a></li>
</ul>

<div class='collapse-nav'>
<a class="toggle-nav-collapse" title="Open/Close" href="#"><i class="fa fa-angle-left"></i></a>

</div>
</div>
<div class='content-wrapper'>
<div class='flash-container'>
</div>


<div class='container-fluid container-limited'>
<div class='content'>
<div class='clearfix'>


<div class='tree-holder' id='tree-holder'>
<div class='nav-block'>
<div class='tree-ref-holder'>
<form class="project-refs-form" action="/G_Dog1985/documentation/refs/switch" accept-charset="UTF-8" method="get"><input name="utf8" type="hidden" value="&#x2713;" />
<select name="ref" id="ref" class="project-refs-select select2 select2-sm"><optgroup label="Branches"><option value="master">master</option>
<option value="revert-122-patch-1">revert-122-patch-1</option></optgroup><optgroup label="Tags"><option value="v1.1">v1.1</option>
<option value="v1.0">v1.0</option>
<option value="v0.9.9">v0.9.9</option>
<option value="v0.9">v0.9</option>
<option value="v0.8">v0.8</option>
<option value="v0.7">v0.7</option>
<option value="v0.6">v0.6</option>
<option value="v0.1.2">v0.1.2</option>
<option value="v0.1.1">v0.1.1</option>
<option value="v0.1.0">v0.1.0</option>
<option value="last-mastodon-state">last-mastodon-state</option></optgroup><optgroup label="Commit"><option selected="selected" value="51f4311fb3451260e888b54ace8e17e8ed9141e1">51f4311fb3451260e888b54ace8e17e8ed9141e1</option></optgroup></select>
<input type="hidden" name="destination" id="destination" value="blob" />
<input type="hidden" name="path" id="path" value="Running-Mastodon/Production-guide.md" />
</form>


</div>
<ul class='breadcrumb repo-breadcrumb'>
<li>
<a href="/G_Dog1985/documentation/tree/51f4311fb3451260e888b54ace8e17e8ed9141e1">documentation
</a></li>
<li>
<a href="/G_Dog1985/documentation/tree/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon">Running-Mastodon</a>
</li>
<li>
<a href="/G_Dog1985/documentation/blob/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md"><strong>
Production-guide.md
</strong>
</a></li>
</ul>
</div>
<ul class='blob-commit-info hidden-xs'>
<li class='commit js-toggle-container' id='commit-51f4311f'>
<div class='commit-row-title'>
<span class='item-title'>
<a class="commit-row-message" href="/G_Dog1985/documentation/commit/51f4311fb3451260e888b54ace8e17e8ed9141e1">Merge branch 'master' of </a><a href="https://github.com/tootsuite/documentation" rel="nofollow noreferrer">https://github.com/tootsuite/documentation</a>
</span>
<div class='pull-right'>
<a class="ci-status-link ci-status-icon-pending" title="Build pending" data-toggle="tooltip" data-placement="auto left" href="/G_Dog1985/documentation/commit/51f4311fb3451260e888b54ace8e17e8ed9141e1/builds"><i class="fa fa-clock-o fa-fw"></i></a>
<button class="btn btn-clipboard" data-clipboard-text="51f4311fb3451260e888b54ace8e17e8ed9141e1" type="button"><i class="fa fa-clipboard"></i></button>
<a class="commit_short_id" href="/G_Dog1985/documentation/commit/51f4311fb3451260e888b54ace8e17e8ed9141e1">51f4311f</a>
</div>
</div>
<div class='commit-row-info'>
by
<a class="commit-author-link has-tooltip" title="greg@localhost.localdomain" href="mailto:greg@localhost.localdomain"><img class="avatar s24" width="24" alt="" src="http://www.gravatar.com/avatar/321615643a50e591215240e2b17bcb83?s=48&amp;d=identicon" /> <span class="commit-author-name">Greg Komula</span></a>
<div class='committed_ago'>
<time class="time_ago js-timeago js-timeago-pending" datetime="2017-11-14T10:57:45Z" title="Nov 14, 2017 10:57am" data-toggle="tooltip" data-placement="top" data-container="body">2017-11-14 05:57:45 -0500</time><script>
//<![CDATA[
$('.js-timeago-pending').removeClass('js-timeago-pending').timeago()
//]]>
</script> &nbsp;
</div>
<a class="pull-right" href="/G_Dog1985/documentation/tree/51f4311fb3451260e888b54ace8e17e8ed9141e1">Browse Files</a>
</div>
</li>

</ul>
<div class='blob-content-holder' id='blob-content-holder'>
<article class='file-holder'>
<div class='file-title'>
<i class="fa fa-file-text-o fa-fw"></i>
<strong>
Production-guide.md
</strong>
<small>
16.5 KB
</small>
<div class='file-actions hidden-xs'>
<div class='btn-group tree-btn-group'>
<a class="btn btn-sm" target="_blank" href="/G_Dog1985/documentation/raw/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md">Raw</a>
<a class="btn btn-sm" href="/G_Dog1985/documentation/blame/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md">Blame</a>
<a class="btn btn-sm" href="/G_Dog1985/documentation/commits/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md">History</a>
<a class="btn btn-sm" href="/G_Dog1985/documentation/blob/51f4311fb3451260e888b54ace8e17e8ed9141e1/Running-Mastodon/Production-guide.md">Permalink</a>
</div>

</div>
</div>
<div class='file-content wiki'>
<h1>&#x000A;<a id="mastodon-production-guide" class="anchor" href="#mastodon-production-guide" aria-hidden="true"></a>Mastodon Production Guide</h1>&#x000A;&#x000A;<p><strong>Disclaimer:</strong></p>&#x000A;&#x000A;<p>This guide was written for <a href="https://www.ubuntu.com/server" rel="nofollow noreferrer">Ubuntu Server 16.04</a>, you may run into issues if you are using another operating system. We are welcoming contributions for guides to other distributions.</p>&#x000A;&#x000A;<p>This document is also written with the expectation that you have a technical level high enough to administrate Linux servers.</p>&#x000A;&#x000A;<p>If you need help setting up your instance, you may want to try tooting at the <a href="https://mastodon.social/tags/mastoadmins" rel="nofollow noreferrer">#MastoAdmins</a> hashtag.</p>&#x000A;&#x000A;<h2>&#x000A;<a id="what-is-this-guide" class="anchor" href="#what-is-this-guide" aria-hidden="true"></a>What is this guide?</h2>&#x000A;&#x000A;<p>This guide is a walk through of the setup process of a <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> instance.</p>&#x000A;&#x000A;<p>We use example.com to represent a domain or sub-domain. Example.com should be replaced with your instance domain or sub-domain.</p>&#x000A;&#x000A;<h2>&#x000A;<a id="prerequisites" class="anchor" href="#prerequisites" aria-hidden="true"></a>Prerequisites</h2>&#x000A;&#x000A;<p>You will need the following for this guide:</p>&#x000A;&#x000A;<ul>&#x000A;<li>A server running <a href="https://www.ubuntu.com/server" rel="nofollow noreferrer">Ubuntu Server 16.04</a>.</li>&#x000A;<li>Root access to the server.</li>&#x000A;<li>A domain or sub-domain to use for the instance.</li>&#x000A;</ul>&#x000A;&#x000A;<h2>&#x000A;<a id="dns" class="anchor" href="#dns" aria-hidden="true"></a>DNS</h2>&#x000A;&#x000A;<p>DNS records should be added before anything is done on the server.</p>&#x000A;&#x000A;<p>The records added are:</p>&#x000A;&#x000A;<ul>&#x000A;<li> A record (IPv4 address) for example.com</li>&#x000A;<li> AAAA record (IPv6 address) for example.com</li>&#x000A;</ul>&#x000A;&#x000A;<blockquote>&#x000A;<h3>&#x000A;<a id="a-helpful-and-optional-note" class="anchor" href="#a-helpful-and-optional-note" aria-hidden="true"></a>A Helpful And Optional Note</h3>&#x000A;&#x000A;<p>Using <code>tmux</code> when following through with this guide will be helpful.</p>&#x000A;&#x000A;<p>Not only will this help you not lose your place if you are disconnected, it will let you have multiple terminal windows open for switching contexts (root user versus the mastodon user).</p>&#x000A;&#x000A;<p>You can install <a href="https://github.com/tmux/tmux/wiki" rel="nofollow noreferrer">tmux</a> from the package manager:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>apt -y install tmux</code></pre>&#x000A;&#x000A;</blockquote>&#x000A;&#x000A;<h2>&#x000A;<a id="dependency-installation" class="anchor" href="#dependency-installation" aria-hidden="true"></a>Dependency Installation</h2>&#x000A;&#x000A;<p>All dependencies should be installed as root.</p>&#x000A;&#x000A;<h3>&#x000A;<a id="nodejs-repository" class="anchor" href="#nodejs-repository" aria-hidden="true"></a>node.js Repository</h3>&#x000A;&#x000A;<p>You will need to add an external repository so we can have the version of <a href="https://nodejs.org/en/" rel="nofollow noreferrer">node.js</a> required.</p>&#x000A;&#x000A;<p>We run this script to add the repository:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>apt -y install curl&#x000A;curl -sL https://deb.nodesource.com/setup_7.x | bash -</code></pre>&#x000A;&#x000A;&#x000A;<p>The <a href="https://nodejs.org/en/" rel="nofollow noreferrer">node.js</a> repository is now added.</p>&#x000A;&#x000A;<h3>&#x000A;<a id="yarn-repository" class="anchor" href="#yarn-repository" aria-hidden="true"></a>Yarn Repository</h3>&#x000A;&#x000A;<p>Another repository needs to be added so we can get the version of <a href="https://yarnpkg.com/en/" rel="nofollow noreferrer">Yarn</a> used by <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a>.</p>&#x000A;&#x000A;<p>This is how you add the repository:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -&#x000A;<span class="nb">echo</span> <span class="s2">"deb https://dl.yarnpkg.com/debian/ stable main"</span> | tee /etc/apt/sources.list.d/yarn.list&#x000A;apt update</code></pre>&#x000A;&#x000A;&#x000A;<h3>&#x000A;<a id="various-other-dependencies" class="anchor" href="#various-other-dependencies" aria-hidden="true"></a>Various Other Dependencies</h3>&#x000A;&#x000A;<p>Now you need to install <a href="https://yarnpkg.com/en/" rel="nofollow noreferrer">Yarn</a> plus some more software.</p>&#x000A;&#x000A;<h4>&#x000A;<a id="explanation-of-the-dependencies" class="anchor" href="#explanation-of-the-dependencies" aria-hidden="true"></a>Explanation of the dependencies</h4>&#x000A;&#x000A;<ul>&#x000A;<li>imagemagick - Mastodon uses imagemagick for image related operations</li>&#x000A;<li>ffmpeg - Mastodon uses ffmpeg for conversion of GIFs to MP4s</li>&#x000A;<li>libprotobuf-dev and protobuf-compiler - Mastodon uses these for language detection</li>&#x000A;<li>nginx - nginx is our frontend web server</li>&#x000A;<li>redis-* - Mastodon uses redis for its in-memory data structure store</li>&#x000A;<li>postgresql-* - Mastodon uses PostgreSQL as it's SQL database</li>&#x000A;<li>nodejs - Node is used for Mastodon's streaming API</li>&#x000A;<li>yarn - Yarn is a Node.js package manager</li>&#x000A;<li>Other -dev packages, g++ - these are needed for the compilation of Ruby using ruby-build.</li>&#x000A;</ul>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>apt -y install imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev nginx redis-server redis-tools postgresql postgresql-contrib letsencrypt yarn libidn11-dev libicu-dev</code></pre>&#x000A;&#x000A;&#x000A;<h3>&#x000A;<a id="dependencies-that-need-to-be-added-as-a-non-root-user" class="anchor" href="#dependencies-that-need-to-be-added-as-a-non-root-user" aria-hidden="true"></a>Dependencies That Need To Be Added As A Non-Root User</h3>&#x000A;&#x000A;<p>Let us create this user first:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>adduser mastodon</code></pre>&#x000A;&#x000A;&#x000A;<p>Log in as the <code>mastodon</code> user:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>sudo su - mastodon</code></pre>&#x000A;&#x000A;&#x000A;<p>We will need to set up <a href="https://github.com/rbenv/rbenv" rel="nofollow noreferrer"><code>rbenv</code></a> and <a href="https://github.com/rbenv/ruby-build" rel="nofollow noreferrer"><code>ruby-build</code></a>:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>git clone https://github.com/rbenv/rbenv.git ~/.rbenv&#x000A;<span class="nb">cd</span> ~/.rbenv <span class="o">&amp;&amp;</span> src/configure <span class="o">&amp;&amp;</span> make -C src&#x000A;<span class="nb">echo</span> <span class="s1">'export PATH="$HOME/.rbenv/bin:$PATH"'</span> &gt;&gt; ~/.bashrc&#x000A;<span class="nb">echo</span> <span class="s1">'eval "$(rbenv init -)"'</span> &gt;&gt; ~/.bashrc&#x000A;<span class="c"># Restart shell</span>&#x000A;<span class="nb">exec </span>bash&#x000A;<span class="c"># Check if rbenv is correctly installed</span>&#x000A;<span class="nb">type </span>rbenv&#x000A;<span class="c"># Install ruby-build as rbenv plugin</span>&#x000A;git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build</code></pre>&#x000A;&#x000A;&#x000A;<p>Now that <a href="https://github.com/rbenv/rbenv" rel="nofollow noreferrer"><code>rbenv</code></a> and <a href="https://github.com/rbenv/ruby-build" rel="nofollow noreferrer"><code>ruby-build</code></a> are installed, we will install the&#x000A;<a href="https://www.ruby-lang.org/en/" rel="nofollow noreferrer">Ruby</a> version which <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> uses. That version will also need to be enabled.</p>&#x000A;&#x000A;<p>To enable <a href="https://www.ruby-lang.org/en/" rel="nofollow noreferrer">Ruby</a>, run:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>rbenv install 2.4.2&#x000A;rbenv global 2.4.2</code></pre>&#x000A;&#x000A;&#x000A;<p><strong>This will take some time. Go stretch for a bit and drink some water while the commands run.</strong></p>&#x000A;&#x000A;<h3>&#x000A;<a id="nodejs-and-ruby-dependencies" class="anchor" href="#nodejs-and-ruby-dependencies" aria-hidden="true"></a>node.js And Ruby Dependencies</h3>&#x000A;&#x000A;<p>Now that <a href="https://www.ruby-lang.org/en/" rel="nofollow noreferrer">Ruby</a> is enabled, we will clone the <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon git repository</a> and install the <a href="https://www.ruby-lang.org/en/" rel="nofollow noreferrer">Ruby</a> and <a href="https://nodejs.org/en/" rel="nofollow noreferrer">node.js</a> dependancies.</p>&#x000A;&#x000A;<p>Run the following to clone and install:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="c"># Return to mastodon user's home directory</span>&#x000A;<span class="nb">cd</span> ~&#x000A;<span class="c"># Clone the mastodon git repository into ~/live</span>&#x000A;git clone https://github.com/tootsuite/mastodon.git live&#x000A;<span class="c"># Change directory to ~live</span>&#x000A;<span class="nb">cd</span> ~/live&#x000A;<span class="c"># </span>&#x000A;<span class="c"># Checkout to the latest stable branch</span>&#x000A;git checkout <span class="k">$(</span>git tag -l | grep -v <span class="s1">'rc[0-9]*$'</span> | sort -V | tail -n 1<span class="k">)</span>&#x000A;<span class="c"># Install bundler</span>&#x000A;gem install bundler&#x000A;<span class="c"># Use bundler to install the rest of the Ruby dependencies</span>&#x000A;bundle install --deployment --without development <span class="nb">test</span>&#x000A;<span class="c"># Use yarn to install node.js dependencies</span>&#x000A;yarn install --pure-lockfile</code></pre>&#x000A;&#x000A;&#x000A;<p>That is all we need to do for now with the <code>mastodon</code> user, you can now <code>exit</code> back to root.</p>&#x000A;&#x000A;<h2>&#x000A;<a id="postgresql-database-creation" class="anchor" href="#postgresql-database-creation" aria-hidden="true"></a>PostgreSQL Database Creation</h2>&#x000A;&#x000A;<p><a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> requires access to a <a href="https://www.postgresql.org" rel="nofollow noreferrer">PostgreSQL</a> instance.</p>&#x000A;&#x000A;<p>Create a user for a <a href="https://www.postgresql.org" rel="nofollow noreferrer">PostgreSQL</a> instance:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code># Launch psql as the postgres user&#x000A;sudo -u postgres psql&#x000A;&#x000A;# In the following prompt&#x000A;CREATE USER mastodon CREATEDB;&#x000A;\q</code></pre>&#x000A;&#x000A;&#x000A;<p><strong>Note</strong> that we do not set up a password of any kind, this is because we will be using ident authentication. This allows local users to access the database without a password.</p>&#x000A;&#x000A;<h2>&#x000A;<a id="nginx-configuration" class="anchor" href="#nginx-configuration" aria-hidden="true"></a>nginx Configuration</h2>&#x000A;&#x000A;<p>You need to configure <a href="http://nginx.org" rel="nofollow noreferrer">nginx</a> to serve your <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> instance.</p>&#x000A;&#x000A;<p><strong>Reminder: Replace all occurrences of example.com with your own instance's domain or sub-domain.</strong></p>&#x000A;&#x000A;<p><code>cd</code> to <code>/etc/nginx/sites-available</code> and open a new file:</p>&#x000A;&#x000A;<p><code>nano /etc/nginx/sites-available/example.com.conf</code></p>&#x000A;&#x000A;<p>Copy and paste the following and make edits as necessary:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight nginx"><code><span class="k">map</span> <span class="nv">$http_upgrade</span> <span class="nv">$connection_upgrade</span> <span class="p">{</span>&#x000A;  <span class="kn">default</span> <span class="s">upgrade</span><span class="p">;</span>&#x000A;  <span class="kn">''</span>      <span class="s">close</span><span class="p">;</span>&#x000A;<span class="p">}</span>&#x000A;&#x000A;<span class="k">server</span> <span class="p">{</span>&#x000A;  <span class="kn">listen</span> <span class="mi">80</span><span class="p">;</span>&#x000A;  <span class="kn">listen</span> <span class="s">[::]:80</span><span class="p">;</span>&#x000A;  <span class="kn">server_name</span> <span class="s">example.com</span><span class="p">;</span>&#x000A;  <span class="c1"># Useful for Let's Encrypt</span>&#x000A;  <span class="kn">location</span> <span class="n">/.well-known/acme-challenge/</span> <span class="p">{</span> <span class="kn">allow</span> <span class="s">all</span><span class="p">;</span> <span class="p">}</span>&#x000A;  <span class="kn">location</span> <span class="n">/</span> <span class="p">{</span> <span class="kn">return</span> <span class="mi">301</span> <span class="s">https://</span><span class="nv">$host$request_uri</span><span class="p">;</span> <span class="p">}</span>&#x000A;<span class="p">}</span>&#x000A;&#x000A;<span class="k">server</span> <span class="p">{</span>&#x000A;  <span class="kn">listen</span> <span class="mi">443</span> <span class="s">ssl</span> <span class="s">http2</span><span class="p">;</span>&#x000A;  <span class="kn">listen</span> <span class="s">[::]:443</span> <span class="s">ssl</span> <span class="s">http2</span><span class="p">;</span>&#x000A;  <span class="kn">server_name</span> <span class="s">example.com</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">ssl_protocols</span> <span class="s">TLSv1.2</span><span class="p">;</span>&#x000A;  <span class="kn">ssl_ciphers</span> <span class="s">HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA</span><span class="p">;</span>&#x000A;  <span class="kn">ssl_prefer_server_ciphers</span> <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="kn">ssl_session_cache</span> <span class="s">shared:SSL:10m</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">ssl_certificate</span>     <span class="n">/etc/letsencrypt/live/example.com/fullchain.pem</span><span class="p">;</span>&#x000A;  <span class="kn">ssl_certificate_key</span> <span class="n">/etc/letsencrypt/live/example.com/privkey.pem</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">keepalive_timeout</span>    <span class="mi">70</span><span class="p">;</span>&#x000A;  <span class="kn">sendfile</span>             <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="kn">client_max_body_size</span> <span class="mi">0</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">root</span> <span class="n">/home/mastodon/live/public</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">gzip</span> <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_disable</span> <span class="s">"msie6"</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_vary</span> <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_proxied</span> <span class="s">any</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_comp_level</span> <span class="mi">6</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_buffers</span> <span class="mi">16</span> <span class="mi">8k</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_http_version</span> <span class="mi">1</span><span class="s">.1</span><span class="p">;</span>&#x000A;  <span class="kn">gzip_types</span> <span class="nc">text/plain</span> <span class="nc">text/css</span> <span class="nc">application/json</span> <span class="nc">application/javascript</span> <span class="nc">text/xml</span> <span class="nc">application/xml</span> <span class="nc">application/xml</span><span class="s">+rss</span> <span class="nc">text/javascript</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">add_header</span> <span class="s">Strict-Transport-Security</span> <span class="s">"max-age=31536000"</span><span class="p">;</span>&#x000A;&#x000A;  <span class="kn">location</span> <span class="n">/</span> <span class="p">{</span>&#x000A;    <span class="kn">try_files</span> <span class="nv">$uri</span> <span class="s">@proxy</span><span class="p">;</span>&#x000A;  <span class="p">}</span>&#x000A;&#x000A;  <span class="kn">location</span> <span class="p">~</span> <span class="sr">^/(emoji|packs|system/accounts/avatars|system/media_attachments/files)</span> <span class="p">{</span>&#x000A;    <span class="kn">add_header</span> <span class="s">Cache-Control</span> <span class="s">"public,</span> <span class="s">max-age=31536000,</span> <span class="s">immutable"</span><span class="p">;</span>&#x000A;    <span class="kn">try_files</span> <span class="nv">$uri</span> <span class="s">@proxy</span><span class="p">;</span>&#x000A;  <span class="p">}</span>&#x000A;&#x000A;  <span class="kn">location</span> <span class="n">/sw.js</span> <span class="p">{</span>&#x000A;    <span class="kn">add_header</span> <span class="s">Cache-Control</span> <span class="s">"public,</span> <span class="s">max-age=0"</span><span class="p">;</span>&#x000A;    <span class="kn">try_files</span> <span class="nv">$uri</span> <span class="s">@proxy</span><span class="p">;</span>&#x000A;  <span class="p">}</span>&#x000A;&#x000A;  <span class="kn">location</span> <span class="s">@proxy</span> <span class="p">{</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Host</span> <span class="nv">$host</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Real-IP</span> <span class="nv">$remote_addr</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Forwarded-For</span> <span class="nv">$proxy_add_x_forwarded_for</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Forwarded-Proto</span> <span class="s">https</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Proxy</span> <span class="s">""</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_pass_header</span> <span class="s">Server</span><span class="p">;</span>&#x000A;&#x000A;    <span class="kn">proxy_pass</span> <span class="s">http://127.0.0.1:3000</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_buffering</span> <span class="no">off</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_redirect</span> <span class="no">off</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_http_version</span> <span class="mi">1</span><span class="s">.1</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Upgrade</span> <span class="nv">$http_upgrade</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Connection</span> <span class="nv">$connection_upgrade</span><span class="p">;</span>&#x000A;&#x000A;    <span class="kn">tcp_nodelay</span> <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="p">}</span>&#x000A;&#x000A;  <span class="kn">location</span> <span class="n">/api/v1/streaming</span> <span class="p">{</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Host</span> <span class="nv">$host</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Real-IP</span> <span class="nv">$remote_addr</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Forwarded-For</span> <span class="nv">$proxy_add_x_forwarded_for</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">X-Forwarded-Proto</span> <span class="s">https</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Proxy</span> <span class="s">""</span><span class="p">;</span>&#x000A;&#x000A;    <span class="kn">proxy_pass</span> <span class="s">http://127.0.0.1:4000</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_buffering</span> <span class="no">off</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_redirect</span> <span class="no">off</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_http_version</span> <span class="mi">1</span><span class="s">.1</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Upgrade</span> <span class="nv">$http_upgrade</span><span class="p">;</span>&#x000A;    <span class="kn">proxy_set_header</span> <span class="s">Connection</span> <span class="nv">$connection_upgrade</span><span class="p">;</span>&#x000A;&#x000A;    <span class="kn">tcp_nodelay</span> <span class="no">on</span><span class="p">;</span>&#x000A;  <span class="p">}</span>&#x000A;&#x000A;  <span class="kn">error_page</span> <span class="mi">500</span> <span class="mi">501</span> <span class="mi">502</span> <span class="mi">503</span> <span class="mi">504</span> <span class="n">/500.html</span><span class="p">;</span>&#x000A;<span class="p">}</span></code></pre>&#x000A;&#x000A;&#x000A;<p>Activate the <a href="http://nginx.org" rel="nofollow noreferrer">nginx</a> configuration added:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="nb">cd</span> /etc/nginx/sites-enabled&#x000A;ln -s ../sites-available/example.com.conf</code></pre>&#x000A;&#x000A;&#x000A;<p>This configuration makes the assumption you are using <a href="https://letsencrypt.org" rel="nofollow noreferrer">Let's Encrypt</a> as your TLS certificate provider.</p>&#x000A;&#x000A;<p><strong>If you are going to be using Let's Encrypt as your TLS certificate provider, see the&#x000A;next sub-section. If not edit the <code>ssl_certificate</code> and <code>ssl_certificate_key</code> values&#x000A;accordingly.</strong></p>&#x000A;&#x000A;<h2>&#x000A;<a id="lets-encrypt" class="anchor" href="#lets-encrypt" aria-hidden="true"></a>Let's Encrypt</h2>&#x000A;&#x000A;<p>This section is only relevant if you are using <a href="https://letsencrypt.org/" rel="nofollow noreferrer">Let's Encrypt</a>&#x000A;as your TLS certificate provider.</p>&#x000A;&#x000A;<h3>&#x000A;<a id="generation-of-the-certificate" class="anchor" href="#generation-of-the-certificate" aria-hidden="true"></a>Generation Of The Certificate</h3>&#x000A;&#x000A;<p>We need to generate Let's Encrypt certificates.</p>&#x000A;&#x000A;<p><strong>Make sure to replace any occurrence of 'example.com' with your Mastodon instance's domain.</strong></p>&#x000A;&#x000A;<p>Make sure that <a href="http://nginx.org" rel="nofollow noreferrer">nginx</a> is stopped at this point:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>systemctl stop nginx</code></pre>&#x000A;&#x000A;&#x000A;<p>We will be creating the certificate twice, once with TLS SNI validation in standalone mode and the second time we will be using the webroot method. This is required due to the way&#x000A;<a href="http://nginx.org" rel="nofollow noreferrer">nginx</a> and the <a href="https://letsencrypt.org/" rel="nofollow noreferrer">Let's Encrypt</a> tool works.</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>letsencrypt certonly --standalone -d example.com</code></pre>&#x000A;&#x000A;&#x000A;<p>After that successfully completes, we will use the webroot method. This requires <a href="http://nginx.org" rel="nofollow noreferrer">nginx</a> to be running:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>systemctl start nginx&#x000A;<span class="c"># The letsencrypt tool will ask if you want issue a new cert, please choose that option</span>&#x000A;letsencrypt certonly --webroot -d example.com -w /home/mastodon/live/public/</code></pre>&#x000A;&#x000A;&#x000A;<h3>&#x000A;<a id="automated-renewal-of-lets-encrypt-certificate" class="anchor" href="#automated-renewal-of-lets-encrypt-certificate" aria-hidden="true"></a>Automated Renewal Of Let's Encrypt Certificate</h3>&#x000A;&#x000A;<p><a href="https://letsencrypt.org/" rel="nofollow noreferrer">Let's Encrypt</a> certificates have a validity period of 90 days.</p>&#x000A;&#x000A;<p>You need to renew your certificate before the expiration date. Not doing so will make users of your instance unable to access the instance and users of other instances unable to federate with yours.</p>&#x000A;&#x000A;<p>We can create a cron job that runs daily to do this:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>nano /etc/cron.daily/letsencrypt-renew</code></pre>&#x000A;&#x000A;&#x000A;<p>Copy and paste this script into that file:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="c">#!/usr/bin/env bash</span>&#x000A;letsencrypt renew&#x000A;systemctl reload nginx</code></pre>&#x000A;&#x000A;&#x000A;<p>Save and exit the file.</p>&#x000A;&#x000A;<p>Make the script executable and restart the cron daemon so that the script runs daily:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>chmod +x /etc/cron.daily/letsencrypt-renew&#x000A;systemctl restart cron</code></pre>&#x000A;&#x000A;&#x000A;<p>That is it. Your server will renew your <a href="https://letsencrypt.org/" rel="nofollow noreferrer">Let's Encrypt</a> certificate.</p>&#x000A;&#x000A;<h2>&#x000A;<a id="mastodon-application-configuration" class="anchor" href="#mastodon-application-configuration" aria-hidden="true"></a>Mastodon Application Configuration</h2>&#x000A;&#x000A;<p>We will configure the Mastodon application.</p>&#x000A;&#x000A;<p>For this we will switch to the <code>mastodon</code> system user:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>sudo su - mastodon</code></pre>&#x000A;&#x000A;&#x000A;<p>Change directory to <code>~live</code> and edit the <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> application configuration:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="nb">cd</span> ~/live&#x000A;cp .env.production.sample .env.production&#x000A;nano .env.production</code></pre>&#x000A;&#x000A;&#x000A;<p>For the purposes of this guide, these are the values to be edited:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code># Your Redis host&#x000A;REDIS_HOST=127.0.0.1&#x000A;# Your Redis port&#x000A;REDIS_PORT=6379&#x000A;# Your PostgreSQL host&#x000A;DB_HOST=/var/run/postgresql&#x000A;# Your PostgreSQL user&#x000A;DB_USER=mastodon&#x000A;# Your PostgreSQL DB name&#x000A;DB_NAME=mastodon_production&#x000A;# Leave DB password empty&#x000A;DB_PASS=&#x000A;# Your DB_PORT&#x000A;DB_PORT=5432&#x000A;&#x000A;# Your instance's domain&#x000A;LOCAL_DOMAIN=example.com&#x000A;# We have HTTPS enabled&#x000A;LOCAL_HTTPS=true&#x000A;&#x000A;# Application secrets&#x000A;# Generate each with `RAILS_ENV=production bundle exec rake secret`&#x000A;PAPERCLIP_SECRET=&#x000A;SECRET_KEY_BASE=&#x000A;OTP_SECRET=&#x000A;&#x000A;# Web Push VAPID keys&#x000A;# Generate with `RAILS_ENV=production bundle exec rake mastodon:webpush:generate_vapid_key`&#x000A;VAPID_PRIVATE_KEY=&#x000A;VAPID_PUBLIC_KEY=&#x000A;&#x000A;# All SMTP details, Mailgun and Sparkpost have free tiers&#x000A;SMTP_SERVER=&#x000A;SMTP_PORT=&#x000A;SMTP_LOGIN=&#x000A;SMTP_PASSWORD=&#x000A;SMTP_FROM_ADDRESS=</code></pre>&#x000A;&#x000A;&#x000A;<p>We now need to set up the <a href="https://www.postgresql.org" rel="nofollow noreferrer">PostgreSQL</a> database for the first time:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="nv">RAILS_ENV</span><span class="o">=</span>production bundle <span class="nb">exec </span>rails db:setup</code></pre>&#x000A;&#x000A;&#x000A;<p>Then we will need to precompile all CSS and JavaScript files:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code><span class="nv">RAILS_ENV</span><span class="o">=</span>production bundle <span class="nb">exec </span>rails assets:precompile</code></pre>&#x000A;&#x000A;&#x000A;<p><strong>The assets precompilation takes a couple minutes, so this is a good time to take another break.</strong></p>&#x000A;&#x000A;<h2>&#x000A;<a id="mastodon-systemd-service-files" class="anchor" href="#mastodon-systemd-service-files" aria-hidden="true"></a>Mastodon systemd Service Files</h2>&#x000A;&#x000A;<p>We will need three <a href="https://github.com/systemd/systemd" rel="nofollow noreferrer">systemd</a> service files for each Mastodon service.</p>&#x000A;&#x000A;<p>Now switch back to the root user.</p>&#x000A;&#x000A;<p>For the <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> web workers service place the following in <code>/etc/systemd/system/mastodon-web.service</code>:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code>[Unit]&#x000A;Description=mastodon-web&#x000A;After=network.target&#x000A;&#x000A;[Service]&#x000A;Type=simple&#x000A;User=mastodon&#x000A;WorkingDirectory=/home/mastodon/live&#x000A;Environment="RAILS_ENV=production"&#x000A;Environment="PORT=3000"&#x000A;ExecStart=/home/mastodon/.rbenv/shims/bundle exec puma -C config/puma.rb&#x000A;ExecReload=/bin/kill -SIGUSR1 $MAINPID&#x000A;TimeoutSec=15&#x000A;Restart=always&#x000A;&#x000A;[Install]&#x000A;WantedBy=multi-user.target</code></pre>&#x000A;&#x000A;&#x000A;<p>For <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> background queue service, place the following in <code>/etc/systemd/system/mastodon-sidekiq.service</code>:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code>[Unit]&#x000A;Description=mastodon-sidekiq&#x000A;After=network.target&#x000A;&#x000A;[Service]&#x000A;Type=simple&#x000A;User=mastodon&#x000A;WorkingDirectory=/home/mastodon/live&#x000A;Environment="RAILS_ENV=production"&#x000A;Environment="DB_POOL=5"&#x000A;ExecStart=/home/mastodon/.rbenv/shims/bundle exec sidekiq -c 5 -q default -q mailers -q pull -q push&#x000A;TimeoutSec=15&#x000A;Restart=always&#x000A;&#x000A;[Install]&#x000A;WantedBy=multi-user.target</code></pre>&#x000A;&#x000A;&#x000A;<p>For the <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> streaming API service place the following in <code>/etc/systemd/system/mastodon-streaming.service</code>:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code>[Unit]&#x000A;Description=mastodon-streaming&#x000A;After=network.target&#x000A;&#x000A;[Service]&#x000A;Type=simple&#x000A;User=mastodon&#x000A;WorkingDirectory=/home/mastodon/live&#x000A;Environment="NODE_ENV=production"&#x000A;Environment="PORT=4000"&#x000A;ExecStart=/usr/bin/npm run start&#x000A;TimeoutSec=15&#x000A;Restart=always&#x000A;&#x000A;[Install]&#x000A;WantedBy=multi-user.target</code></pre>&#x000A;&#x000A;&#x000A;<p>Now you need to enable all of these services:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>systemctl <span class="nb">enable</span> /etc/systemd/system/mastodon-<span class="k">*</span>.service</code></pre>&#x000A;&#x000A;&#x000A;<p>Now start the services:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>systemctl start mastodon-<span class="k">*</span>.service</code></pre>&#x000A;&#x000A;&#x000A;<p>Check that they are properly running:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight shell"><code>systemctl status mastodon-<span class="k">*</span>.service</code></pre>&#x000A;&#x000A;&#x000A;<h2>&#x000A;<a id="email-service" class="anchor" href="#email-service" aria-hidden="true"></a>Email Service</h2>&#x000A;&#x000A;<p>If you plan on receiving email notifications or running more than just a single-user instance, you likely will want to get set up with an email provider.</p>&#x000A;&#x000A;<p>There are several free email providers out there- a couple of decent ones are Mailgun.com, which requires a credit card but gives 10,000 free emails, and Sparkpost.com, which gives 15,000 with no credit card but requires you not be on a .space tld.</p>&#x000A;&#x000A;<p>It may be easier to use a subdomain to setup your email with a custom provider - in this case, when registering your domain with the email service, sign up as something like "mail.domain.com"</p>&#x000A;&#x000A;<p>Once you create your account, follow the instructions each provider gives you for updating your DNS records.  Once you have all the information ready to go and the service validates your DNS configuration, edit your config file.  These records should already exist in the configuration, but here's a sample setup that uses Mailgun that you can replace with your own personal info:</p>&#x000A;&#x000A;<p>SMTP_SERVER=smtp.mailgun.org&#x000A;SMTP_PORT=587&#x000A;SMTP_LOGIN=anAccountThatIsntPostmaster@mstdn.domain.com&#x000A;SMTP_PASSWORD=HolySnacksAPassword&#x000A;SMTP_FROM_ADDRESS=Domain.com Mastodon Admin <a href="mailto:notifications@domain.com">notifications@domain.com</a></p>&#x000A;&#x000A;<p>Finally, to test this, spin up a Rails console (see <a href="https://github.com/tootsuite/documentation/blob/master/Running-Mastodon/Administration-guide.md" rel="nofollow noreferrer">the administration guide</a>) and run the following commands to test this out:</p>&#x000A;&#x000A;<pre class="code highlight js-syntax-highlight plaintext"><code>m = UserMailer.new.mail to:'email@address.com', subject: 'test', body: 'awoo'&#x000A;m.deliver</code></pre>&#x000A;&#x000A;&#x000A;<p>That is all! If everything was done correctly, a <a href="https://github.com/tootsuite/mastodon/" rel="nofollow noreferrer">Mastodon</a> instance will appear when you visit <code>https://example.com</code> in a web browser.</p>&#x000A;&#x000A;<p>Congratulations and welcome to the fediverse!</p>
</div>

</article>
</div>

</div>

</div>
</div>
</div>
</div>
</div>



</body>
</html>
<div id='turnkey-credit' style='font-size:10px;text-align:center;padding-top:20px'><a href='https://www.turnkeylinux.org/gitlab'>GitLab Appliance</a> - Powered by <a href='https://www.turnkeylinux.org'>TurnKey Linux</a></div>

