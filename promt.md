Multi-file edit: InitialMemoPDF.tsx + FinalMemoPDF.tsx

Change the blue banner header VALUES font size from 10 to 11 (client request). 
The blue highlighted values use styles.headerValue (fontSize currently 10).

In BOTH memos, change styles.headerValue fontSize from 10 to 11:
    headerValue: {
      color: colors.bannerText,
      fontSize: 11,   // was 10
      flexGrow: 1, flexShrink: 1, flexBasis: 0, minWidth: 0, maxWidth: "100%",
      lineHeight: 1.15
    }

CONSTRAINTS:
- ONLY change headerValue fontSize 10 -> 11, in both memos.
- Do NOT change the labels (headerLabel), title, or anything else.
- Apply to BOTH InitialMemoPDF.tsx and FinalMemoPDF.tsx.
- Only edit these two files. Show the headerValue change in both.
