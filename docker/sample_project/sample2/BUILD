cc_library(
  name = "sample2",
  srcs = ["sample2.cc"],
  hdrs = ["sample2.h"],
)

cc_test(
  name = "sample2_unittest",
  size = "small",
  srcs = ["test/sample2_unittest.cc"],
  deps = [
    "@com_google_googletest//:gtest_main",
    "//sample2:sample2",
  ],
)
