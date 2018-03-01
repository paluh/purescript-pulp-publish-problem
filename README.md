It is impossible to run `pulp docs` or `pulp publish` on this simple project.

The error is related to... `newtype deriving` in `Quickcheck.Gen` for `Gen`:

  ``` purescript
    No type class instance was found for

      Control.Monad.Rec.Class.MonadRec (State
                                          { newSeed :: Seed
                                          , size :: Int
                                          }
                                       )
  ```

But if we change `Gen` definition from

  ``` purescript
  newtype Gen a = Gen (State GenState a)
  ```
to

  ``` purescript
  newtype Gen a = Gen (StateT GenState Identity a)
  ```

it works fine...
