Apply the diff exactly as shown. Add the decodeHtmlEntities helper and wrap only the two VIEW <div> values (internalNotes, externalNotes) with it. Leave edit textareas, save logic, and all other code unchanged. Apply now and confirm.
function decodeHtmlEntities(s: string | null | undefined): string {
  if (!s) return "";
  return s
    .replace(/&quot;/g, '"')
    .replace(/&#39;/g, "'")
    .replace(/&apos;/g, "'")
    .replace(/&lt;/g, "<")
    .replace(/&gt;/g, ">")
    .replace(/&amp;/g, "&");
}
