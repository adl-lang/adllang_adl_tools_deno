# ADL Code Generators Writen in Typescript

Note:
- `adlc` required as a pre-request.

A small wrapper script can be used to turn this into a cli.

e.g.
```typescript
// gen-sql.ts
import {
  genCreateSqlSchema,
} from "https://deno.land/x/adllang_adl_tools_deno@v0.1/mod.ts";
// } from "../adl-tools-deno/mod.ts";
import { parseArgs } from "https://deno.land/std@0.224.0/cli/parse_args.ts";

async function main() {

  const flags = parseArgs(Deno.args, {
    collect: [
      "searchdir",
    ],
    string: [
      "outputdir",
      "verbose",
      "createFile",
      "viewsFile",
      "metadataFile",
    ],
    boolean: [
      "verbose",
    ],
    default: {
      verbose: false,
    },
  });

  //TODO check for manditory flags, usage
  await genCreateSqlSchema({
      searchPath: flags.searchdir,
      createFile: flags.createFile,
      viewsFile: flags.viewsFile,
      metadataFile: flags.metadataFile,
      adlFiles: flags._,
      verbose: flags.verbose,
  });
}

main()
  .catch((err) => {
    console.error("error in main", err);
  });
```

You can then use deno to run this from the command line. e.g.
```bash
mkdir -p sql/create
deno run --allow-write --allow-run --allow-read \
  ./gen-sql.ts \
  --searchdir=adl \
  --searchdir=../../packages/helix-common/adl \
  --outputdir=sql \
  --createFile=sql/create/V01__create.sql \
  --viewsFile=sql/create/V02__views.sql \
  --metadataFile=sql/create/V03__metadata.sql \
  `find adl -name "*.adl"`
```