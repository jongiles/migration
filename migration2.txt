import os, sys, subprocess

def get_paths(filename='paths.txt'):
   with open(filename) as fp:
      for line in fp:
         line = line.strip()
         src_path,dst_path=line.split()	
         if line:
               yield src_path, dst_path
       
def call(*args):
    process = subprocess.Popen(args, stdout=subprocess.PIPE)
    output, err = process.communicate()
    exit_code = process.wait()
    return output, err, exit_code


def start_cp(src_path,dst_path):
    output, err, exit_code = call('/bin/cp', '-r', src_path, dst_path)
    if exit_code:
      #print('FAILED to cp ', src_path, 'to ', dst_path, exit_code, err, file=sys.stderr)
      print'FAILED to cp ', src_path, 'to ', dst_path, exit_code, err
    return output


if __name__ == '__main__':
   src_dst_paths=list(get_paths())
   for item in src_dst_paths:
      src_path, dst_path=item
      print 'Copy from ', src_path, 'to ', dst_path		
      print(start_cp(src_path, dst_path))
      
