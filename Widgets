#include "cs136.h"

const int DEFAULT_BIN_SIZE = 10;
const int DEFAULT_COGS_REQUIRED = 2;
const int DEFAULT_SPROCKETS_REQUIRED = 3;
/////////////////////////////////////////////////////////////////////////////

// MACHINE STRUCTURE:
// specify the details of the machine structure here
// We provided widget_count as an example (because we implemented empty_bin)
// but you may change that field name if you wish.

struct machine {
  int id;
  int widgets;
  int sprockets;
  int cogs;
  int cogs_min;
  int sprocks_min;
  int bin_size;
};

// empty_bin(m) removes all widgets from the completed bin of m
// effects: modifies *m

void empty_bin(struct machine *m) {
  assert(m);
  m->widgets = 0;
}


// reset_machine(m) resets machine m back to its initial settings
// effects: modifies *m

void reset_machine(struct machine *m) {
  assert(m);
  m->cogs = 0;
  m->sprockets = 0;
  m->widgets = 0;
  m->bin_size = 10;
}


// display_status(m) displays the current state of the machine m
// effects: produces output

void display_status(const struct machine *m) {
  assert(m);
  printf("number of cogs: %d\n", m->cogs);
  printf("number of sprockets: %d\n", m->sprockets);
  printf("completed bin contains %d of %d widgets\n", m->widgets, m->bin_size);
  // Strings to use for printed messages (in this order)
  // "number of cogs: %d\n"
  // "number of sprockets: %d\n"
  // "completed bin contains %d of %d widgets\n"
}


// insert_sprocket(m, amount) adds amount sprockets to the component supply
//   of machine m if amount is valid
// effects: may modify *m
//          may produce output

void insert_sprocket(struct machine *m, int amount) {
  assert(m);
  if (amount > 0) {
    m->sprockets = m->sprockets + amount;
  } else {
    printf("Error: must insert a positive number of sprockets\n");
  }
  // String to use for error message
  // "Error: must insert a positive number of sprockets\n"
}


// insert_cog(m, amount) adds amount sprockets to the component supply
//   of machine m if amount is valid
// effects: may modify *m
//          may produce output

void insert_cog(struct machine *m, int amount) {
  assert(m);
  if (amount > 0) {
    m->cogs = m->cogs + amount;
  } else {
    printf("Error: must insert a positive number of cogs\n");
  }
  // String to use for error message
  // "Error: must insert a positive number of cogs\n"
}


// display_config(m) prints the id of the machine m and its configuration
// effects: produces output

void display_config(const struct machine *m) {
  assert(m);
  printf("Machine id = %d\n", m->id);
  printf("Machine Config: requires %d cogs and %d sprockets, bin size = %d\n"
         , m->cogs_min, m->sprocks_min, m->bin_size);
  // Strings to use for printed messages (in this order)
  // "Machine id = %d\n"
  // "Machine Config: requires %d cogs and %d sprockets, bin size = %d\n"
}


int min_of_3(int x, int y, int z) {
  if (z <= y && z <= x) {
    return z;
  } else if (x <= y) {
    return x;
  } else {
    return y;
  }
}

// make(m) starts machine m and makes as many widgets as possible 
//   before it runs out of supply or the completed bin is full
// effects: may modify *m
//          may produce output

void make(struct machine *m) {
  assert(m);
  const int minimum = min_of_3(m->bin_size - m->widgets,
                               m->cogs / m->cogs_min,
                               m->sprockets / m->sprocks_min);

  m->widgets += minimum;
  m->cogs -= minimum * m->cogs_min;
  m->sprockets -= minimum * m->sprocks_min;

  if (m->bin_size == m->widgets) {
    printf("Completed bin is full\n");
  } 
  if (m->cogs < m->cogs_min) {
    printf("Out of cogs\n");
  } 
  if (m->sprockets < m->sprocks_min) {
    printf("Out of sprockets\n");
  } 

  // Strings to use  (in this order)
  // "Out of cogs\n"
  // "Out of sprockets\n"
  // "Completed bin is full\n"

}


// replace_bin(m, new_size) replaces the bin of machine m with a new one
//   with different capacity (new_size) if new_size is valid
// effects: may modify *m
//          may produce output

void replace_bin(struct machine *m, int new_size) {
  assert(m);
  if (new_size <= 0) {
    printf("Error: completed bin must have positive size\n");
  } else if (m->widgets != 0) {
    printf("Error: completed bin must be empty\n");
  } else {
    m->bin_size = new_size;
  }
  // Strings to use for errors (in this order)
  // "Error: completed bin must have positive size\n"
  // "Error: completed bin must be empty\n"
}


// modify_machine(m, cogs, sprockets) changes the configuration of
//   machine m to now use the specified number of cogs and sprockets
//   to build a widget (if cogs, sprockets are both positive)
// effects: may modify *m
//          may produce output

void modify_machine(struct machine *m, int cogs, int sprockets) {
  assert(m);
  if (cogs <= 0 && sprockets <= 0) {
    printf("Error: must use a positive number for new required cogs\n");
    printf("Error: must use a positive number for new required sprockets\n");
  } else if (cogs <= 0) {
    printf("Error: must use a positive number for new required cogs\n");
  } else if (sprockets <= 0) {
    printf("Error: must use a positive number for new required sprockets\n");
  } else {
    m->cogs_min = cogs;
    m->sprocks_min = sprockets;
  }
}
// Strings to use for errors (in this order)
// "Error: must use a positive number for new required cogs\n"
// "Error: must use a positive number for new required sprockets\n"


// io_test() reads commands and executes those commands by calling
//    the appropriate function.
// effects: reads input
//          produces output

void io_test(void) {
  // Do not change these constants:
  int RESET = lookup_symbol("reset");
  int STATUS = lookup_symbol("status");
  int COG = lookup_symbol("cog");
  int SPROCKET = lookup_symbol("sprocket");
  int MAKE = lookup_symbol("make");
  int EMPTY = lookup_symbol("empty");
  int RESIZE = lookup_symbol("resize");
  int MODIFY = lookup_symbol("modify");
  int SWITCH = lookup_symbol("switch");
  int CONFIG = lookup_symbol("config");
  int QUIT = lookup_symbol("quit");  

  int cmd = 0;
  int param1 = 0;
  int param2 = 0;

  // Initialize two machines to use default bin size and to require
  // defaults cogs and sprockets.
  struct machine m1 = {1, 0, 0, 0, 2, 3, 10};
  struct machine m2 = {2, 0, 0, 0, 2, 3, 10};

  // DO NOT CHANGE THIS LINE
  struct machine *current_machine = &m1;

  do {
    cmd = read_symbol();
    if (cmd == EMPTY) {
      empty_bin(current_machine);
    } else if (cmd == COG) {
      if (scanf("%d", &param1) != 1) {
        break;
      }
      insert_cog(current_machine, param1);
    } else if (cmd == MAKE) {
      make(current_machine);
    } else if (cmd == SPROCKET) {
      if (scanf("%d", &param1) != 1) {
        break;
      }
      insert_sprocket(current_machine, param1);
    } else if (cmd == RESET) {
      reset_machine(current_machine);
    } else if (cmd == STATUS) {
      display_status(current_machine);
    } else if (cmd == RESIZE) {
      if (scanf("%d", &param1) != 1) {
        break;
      }
      replace_bin(current_machine, param1);
    } else if (cmd == MODIFY) {
      if (scanf("%d", &param1) != 1 || scanf("%d", &param2) != 1) {
        break;
      }
      modify_machine(current_machine, param1, param2);
    } else if (cmd == SWITCH) {
      if (current_machine->id == 1) {
        current_machine = &m2;
      } else {
        current_machine = &m1;
      }
    } else if (cmd == CONFIG) {
      display_config(current_machine);
    } 
  } while ((cmd != QUIT) && (cmd != INVALID_SYMBOL));
}


int main(void) {
  io_test();
}
