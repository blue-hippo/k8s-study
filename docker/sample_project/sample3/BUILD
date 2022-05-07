cc_library(
  name = "sample3",
  hdrs = ["sample3-inl.h"],
)

cc_test(
  name = "sample3_unittest",
  size = "small",
  srcs = ["test/sample3_unittest.cc"],
  deps = [
    "@com_google_googletest//:gtest_main",
    "//sample3:sample3",
  ],
)
