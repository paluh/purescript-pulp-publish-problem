It is impossible to run `pulp docs` or `pulp publish` on this simple project.

The error is related to... `newtype deriving` in `Quickcheck.Gen` for `Gen`:

  ``` purescript
  Error found:
  in module Test.QuickCheck.Gen
  at purescript-pulp-publish-problem/bower_components/purescript-quickcheck/src/Test/QuickCheck/Gen.purs line 75, column 16 - line 75, column 52

    No type class instance was found for

      Control.Monad.Rec.Class.MonadRec (State
                                          { newSeed :: Seed
                                          , size :: Int
                                          }
                                       )


  while checking that expression #dict MonadRec
    has type { tailRecM :: forall a b. (a -> Gen (Step a b)) -> a -> Gen b
             , "Monad0" :: {}
                           -> { "Applicative0" :: {}
                                                  -> { pure :: forall a. a -> Gen a
                                                     , "Apply0" :: {}
                                                                   -> { apply :: forall a b. Gen (a -> b) -> Gen a -> Gen b
                                                                      , "Functor0" :: {}
                                                                                      -> { map :: forall a b. (a -> b) -> Gen a -> Gen b
                                                                                         }
                                                                      }
                                                     }
                              , "Bind1" :: {}
                                           -> { bind :: forall a b. Gen a -> (a -> Gen b) -> Gen b
                                              , "Apply0" :: {}
                                                            -> { apply :: forall a b. Gen (a -> b) -> Gen a -> Gen b
                                                               , "Functor0" :: {}
                                                                               -> { map :: forall a b. (a -> b) -> Gen a -> Gen b
                                                                                  }
                                                               }
                                              }
                              }
             }
  in value declaration monadRecGen
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
