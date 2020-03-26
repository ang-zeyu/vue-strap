<template>
  <ul class="px-0 site-nav-list">
    <li
      v-for="entry in versions"
      :key="entry.name"
      class="mt-2">
      <a :href="entry.baseUrl" :class="['site-nav__a', { 'current': entry.highlight }]">{{ entry.name }}</a>
    </li>
  </ul>
</template>

<script>
const versionsRegex = /\/versions\/([^/]+?)\//;

export default {
  data() {
    return {
      versions: {
        type: Array,
        default: [],
      },
    };
  },

  mounted() {
    // eslint-disable-next-line no-undef
    const fetchUrl = `${currentVersionBaseUrl}/versionsData.json`;
    fetch(fetchUrl).then(res => res.json())
      .then((result) => {
        // For highlighting current page being viewed
        let versionHighlighted = false;

        // Push dummy object for current version first
        const highlightedVersions = [{}];

        const match = window.location.href.match(versionsRegex);
        const versionMatch = (match && match.length === 2) && match[1];

        Object.entries(result.versions).forEach(([name, value]) => {
          const entry = Object.assign({}, value, {
            name,
            highlight: false,
          });

          if (name === versionMatch) {
            versionHighlighted = true;
            entry.highlight = true;
          }

          highlightedVersions.push(entry);
        });

        // Set current version
        highlightedVersions[0] = Object.assign({}, result.current, {
          name: 'Current',
          highlight: !versionHighlighted,
        });

        this.versions = highlightedVersions;
      });
  },
};
</script>
