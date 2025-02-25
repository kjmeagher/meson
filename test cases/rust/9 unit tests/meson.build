project('rust unit tests', 'rust', meson_version: '>=1.2.0')

t = executable(
  'rust_test',
  ['test.rs'],
  rust_args : ['--test'],
)

test(
  'rust test (should fail)',
  t,
  protocol : 'rust',
  suite : ['foo'],
  should_fail : true,
)

test(
  'rust test (should pass)',
  t,
  args : ['--skip', 'test_add_intentional_fail'],
  protocol : 'rust',
  suite : ['foo'],
)


test(
  'rust test (should skip)',
  t,
  args : ['--skip', 'test_add'],
  protocol : 'rust',
  suite : ['foo'],
)

rustdoc = find_program('rustdoc', required: false)
if rustdoc.found()
  # rustdoc is invoked mostly like rustc.  This is a simple example
  # where it is easy enough to invoke it by hand.
  test(
    'rust doctest',
    rustdoc,
    args : ['--test', '--crate-name', 'doctest1', '--crate-type', 'lib', files('doctest1.rs')],
    protocol : 'rust',
    suite : ['doctests'],
  )
endif

exe = executable('rust_exe', ['test2.rs', 'test.rs'], build_by_default : false)

rust = import('rust')
rust.test('rust_test_from_exe', exe, should_fail : true)

lib = static_library('rust_static', ['test.rs'], build_by_default : false, rust_crate_type : 'lib')
rust.test('rust_test_from_static', lib, args: ['--skip', 'test_add_intentional_fail'])

lib = shared_library('rust_shared', ['test.rs'], build_by_default : false)
rust.test('rust_test_from_shared', lib, args: ['--skip', 'test_add_intentional_fail'])

helper = static_library('helper', 'helper.rs')
lib = static_library('rust_link_with', 'test3.rs', build_by_default : false)
rust.test('rust_test_link_with', lib, link_with : helper, rust_args : ['--cfg', 'broken="false"'])
