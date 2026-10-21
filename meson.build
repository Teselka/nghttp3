project(
    'nghttp3',
    'c',
    default_options: [
        'b_ndebug=true'
    ],
    version: '1.15.0',
    meson_version: '>=1.1.0'
)

project_version = meson.project_version()
cc = meson.get_compiler('c')

sources = files(
    'lib/nghttp3_rcbuf.c',
    'lib/nghttp3_mem.c',
    'lib/nghttp3_str.c',
    'lib/nghttp3_conv.c',
    'lib/nghttp3_buf.c',
    'lib/nghttp3_ringbuf.c',
    'lib/nghttp3_pq.c',
    'lib/nghttp3_map.c',
    'lib/nghttp3_ksl.c',
    'lib/nghttp3_qpack.c',
    'lib/nghttp3_qpack_huffman.c',
    'lib/nghttp3_qpack_huffman_data.c',
    'lib/nghttp3_err.c',
    'lib/nghttp3_debug.c',
    'lib/nghttp3_conn.c',
    'lib/nghttp3_stream.c',
    'lib/nghttp3_frame.c',
    'lib/nghttp3_tnode.c',
    'lib/nghttp3_vec.c',
    'lib/nghttp3_gaptr.c',
    'lib/nghttp3_idtr.c',
    'lib/nghttp3_range.c',
    'lib/nghttp3_http.c',
    'lib/nghttp3_version.c',
    'lib/nghttp3_balloc.c',
    'lib/nghttp3_opl.c',
    'lib/nghttp3_objalloc.c',
    'lib/nghttp3_unreachable.c',
    'lib/nghttp3_settings.c',
    'lib/nghttp3_callbacks.c',
    'lib/nghttp3_ratelim.c',
    'lib/sfparse/sfparse.c',
)

includes = include_directories('lib/includes/')

cdata = configuration_data()
cdata.set('PACKAGE_VERSION', project_version)

version_parts = project_version.split('.')

version_major = version_parts[0].to_int()
version_minor = version_parts[1].to_int()
version_patch = version_parts[2].to_int()

version_num = version_major * 256 * 256 + version_minor * 256 + version_patch

cdata.set('PACKAGE_VERSION_NUM', '@0@'.format(version_num))

version_h = configure_file(
    configuration: cdata,
    input: 'lib/includes/nghttp3/version.h.in',
    output: 'version.h'
)

sources += version_h

defs = []

foreach header : [
    'arpa/inet.h',
    'netinet/in.h',
    'byteswap.h',
    'endian.h',
]
    if cc.has_header(header)
        defs += '-DHAVE_@0@'.format(header.underscorify().to_upper())
    endif
endforeach

if get_option('default_library') == 'static'
    defs += '-DNGHTTP3_STATICLIB'
endif

if get_option('debug_output')
    defs += '-DDEBUGBUILD'
endif

nghttp3_lib = library(
    'nghttp3',
    sources,
    include_directories: includes,
    c_args: defs
)

nghttp3_dep = declare_dependency(
    link_with: nghttp3_lib,
    include_directories: includes
)

if get_option('examples')
    add_languages('cpp', native: false)

    example_sources = files(
        'examples/qpack.cc',
        'examples/qpack_encode.cc',
        'examples/qpack_decode.cc',
        'examples/util.cc',
    )

    executable(
        'qpack',
        example_sources,
        dependencies: nghttp3_dep
    )
endif
