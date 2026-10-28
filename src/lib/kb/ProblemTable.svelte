<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const merged = uniqueById(reports as Report[]).filter((r) => r.mergedInto === 4471);
  const oneDay = merged.filter((r) => notes.oneDayMove.includes(r.id));
  const rows = [
    { problem: 'parser flips day and month', n: merged.filter((r) => causeOf(r, tenantOf(r)) === 'parser').length },
    { problem: 'booking-v3 form hint says MM/DD', n: merged.filter((r) => causeOf(r, tenantOf(r)) === 'hint').length },
    { problem: 'booking moves by one day', n: oneDay.length },
  ];
</script>

<p><strong>{merged.length}</strong> reports merged into #4471 · <strong>{rows.length}</strong> problems</p>
<table>
  <thead><tr><th>problem</th><th>reports</th></tr></thead>
  <tbody>
    {#each rows as r (r.problem)}<tr><td>{r.problem}</td><td>{r.n}</td></tr>{/each}
  </tbody>
</table>
<p class="note">Counted where the desk files them, by tenant. The {oneDay.length} one-day moves are filed under the parser and are also listed as their own problem.</p>
