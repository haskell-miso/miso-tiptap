🚀 miso-tiptap
====================

A [miso](https://github.com/dmjio/miso) `Component` for [TipTap](https://tiptap.dev/).

This sample `Component` showcases [tailwind](https://tailwindcss.com/) integration as well.

```haskell
-----------------------------------------------------------------------------
{-# LANGUAGE CPP               #-}
{-# LANGUAGE MultilineStrings  #-}
{-# LANGUAGE LambdaCase        #-}
{-# LANGUAGE OverloadedStrings #-}
-----------------------------------------------------------------------------
module Main where
-----------------------------------------------------------------------------
import           Miso
import           Miso.Html.Element as H
import           Miso.Html.Property as P
-----------------------------------------------------------------------------
#ifdef WASM
foreign export javascript "hs_start" main :: IO ()
#endif
-----------------------------------------------------------------------------
main :: IO ()
main = run $ startApp editor
-----------------------------------------------------------------------------
type Model = ()
type Action = ()
-----------------------------------------------------------------------------
editor :: Component parent Model Action
editor = (component () noop viewModel)
  { scripts = [ Src "https://cdn.tailwindcss.com?plugins=typography" ]
  }
-----------------------------------------------------------------------------
viewModel :: Model -> View Model Action
viewModel () = H.div_
  []
  [ H.div_
    [ P.class_ "element" ]
    [ ]
  , H.script_
    [ type_ "module" ] 
    bootstrapEditor
  ]

bootstrapEditor :: MisoString
bootstrapEditor = 
  """
  import { Editor } from 'https://esm.sh/@tiptap/core'
  import StarterKit from 'https://esm.sh/@tiptap/starter-kit'

  const editor = new Editor({
    element: document.querySelector('.element'),
    extensions: [StarterKit],
    editorProps: {
      attributes: {
        class: 'prose prose-sm sm:prose-base lg:prose-lg xl:prose-2xl m-5 focus:outline-none',
      },
    },
    content: `
    <h2>
      Hi there from miso 🍜 !
    </h2>
    <p>
      this is a basic <em>basic</em> example of <strong>Tiptap</strong>. Sure, there are all kind of basic text styles you’d probably expect from a text editor. But wait until you see the lists:
    </p>
    <ul>
      <li>
        That’s a bullet list with one …
      </li>
      <li>
        … or two list items.
      </li>
    </ul>
    <p>
      Isn’t that great? And all of that is editable. But wait, there’s more. Let’s try a code block:
    </p>
<pre><code class="language-css">body {
  display: none;
}</code></pre>
    <p>
      I know, I know, this is impressive. It’s only the tip of the iceberg though. Give it a try and click a little bit around. Don’t forget to check the other examples too.
    </p>
    <blockquote>
      Wow, that’s amazing. Good work, boy! 👏
      <br />
      — Mom
    </blockquote>
  `,
  })
  """
```
