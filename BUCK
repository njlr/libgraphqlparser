def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

def generate_ast_file(type, name):
  genrule(
    name = name,
    out = name,
    srcs = glob([
      'ast/*.py',
      'ast/ast.ast',
    ]),
    cmd = 'python $SRCDIR/ast/ast.py ' + type + ' $SRCDIR/ast/ast.ast > $OUT',
  )
  return ':' + name

def generate_ast_file_subdir(type, name, subdir):
  genrule(
    name = name,
    out = 'out',
    srcs = glob([
      'ast/*.py',
      'ast/ast.ast',
    ]),
    cmd = 'mkdir -p $OUT/' + subdir + ' && cd $OUT/' + subdir + ' && ' +
          'python $SRCDIR/ast/ast.py ' + type + ' $SRCDIR/ast/ast.ast > $OUT/' + subdir + '/' + name,
  )
  return ':' + name

cxx_library(
  name = 'libgraphqlparser',
  header_namespace = '',
  exported_headers = merge_dicts(subdir_glob([
    ('', 'c/*.h'),
    ('', '*.h'),
    ('', '*.hh'),
  ]), {
    'Ast.h': generate_ast_file('cxx', 'Ast.h'),
    'AstVisitor.h': generate_ast_file('cxx_visitor', 'AstVisitor.h'),
    'c/GraphQLAst.h': generate_ast_file('c', 'GraphQLAst.h'),
    'c/GraphQLAstForEachConcreteType.h': generate_ast_file('c_visitor_impl', 'GraphQLAstForEachConcreteType.h'),
  }),
  srcs = glob([
    'c/*.cpp',
    '*.cpp',
    generate_ast_file_subdir('c_impl', 'GraphQLAst.cpp', 'c') + '/**/*.cpp',
  ]) + [
    generate_ast_file('cxx_impl', 'Ast.cpp'),
  ],
  compiler_flags = [
    '-std=gnu++11',
  ],
  visibility = [
    'PUBLIC',
  ],
)
